# Browser Capture Guidance

Use this reference when automating Rufus Q&A capture through a browser, CDP, Playwright, or another UI automation layer.

For login-required capture, also follow [account-safety.md](account-safety.md).

## Login State Gate

Before reading Listing content or searching for Rufus, verify that the active marketplace browser session is logged into an Amazon buyer account. Use browser-visible signals such as a sign-in page, a "Hello, sign in" header, an account sign-in call-to-action, or a login redirect as evidence of `login_status=not_logged_in`.

If the browser is not logged in:

1. Stop all Rufus, Listing, and tab-scanning actions immediately.
2. Save a blocker state with `login_status=not_logged_in`, `challenge_type=login_required`, and `failure_reason=amazon_buyer_login_required`.
3. Send a normal user-facing message asking for the Amazon buyer account login method, including account/password and any OTP/TOTP workflow, through the agent's approved secret or chat channel.
4. Do not use terminal stdin, `input()`, or background prompts.
5. Close the capture-owned browser session unless the user is actively completing a manual login in that session.

If a pre-authorized login workflow exists, use it once, verify `login_status=logged_in`, and continue. If login still fails, save the blocker and stop instead of retrying in a loop.

## Mobile Number Verification Gate

If Amazon displays an "Add a mobile number", "Add mobile number", phone number, or mobile verification page after login, treat it as an account security verification blocker, even if username/password login already succeeded. Do not keep looking for Rufus while this page is active.

Required flow:

1. Stop Rufus, Listing, tab-scanning, and retry-loop actions immediately.
2. Save blocker state with `challenge_type=mobile_number_required`, `failure_reason=mobile_number_verification_required`, and `verification_action=human_intervention_needed`.
3. Ask the human for the Amazon buyer account phone number through the normal user-facing or approved secret channel.
4. Enter the phone number only after the human provides it, submit the form, and do not persist or print the phone number.
5. After the form is accepted, send the exact user-facing status `已添加手机号`.
6. When Amazon asks for the SMS verification code, ask the human for the code through the normal user-facing or approved secret channel.
7. Enter the SMS code only for this verification attempt, submit it, and do not persist or print the code.
8. Confirm the page is no longer on the mobile verification screen and the browser is still logged into the buyer account.
9. Resume from `login_state_check` and continue the original ASIN capture plan.

If the phone number is rejected, the SMS code is rejected, the code expires, or Amazon shows an account security warning, save a blocker and stop. Do not retry indefinitely.

## Pre-Capture Listing Snapshot

Before interacting with Rufus, save the visible Listing context needed for product profiling:

- title, brand, ASIN, price, rating, review count, and variants,
- bullets and product details,
- visible image callouts and A+ sections when accessible,
- existing Q&A and review themes when visible.

Use this snapshot to create the product profile and planned question pool. If the automation cannot read a section, record it as unavailable instead of guessing.

## Login and Verification Check

If Rufus is not visible until login, check for a pre-authorized login and OTP/TOTP workflow before asking the user for help. Use the workflow only through approved secret channels and record non-sensitive status fields such as `logged_in=true` or `verification_action=preauthorized_otp_used`.

If there is no pre-authorized workflow, or if Amazon presents an account security warning, pause and ask for human assistance. Do not print credentials, cookies, OTP seeds, or one-time codes.

Do not ask for login help, verification codes, overwrite decisions, or other confirmations through terminal stdin or blocking `input()` prompts. In background or remote runs, use the agent's normal user-facing reply, or save a blocker state and exit cleanly.

## Submit Button Verification

Before sending questions, verify the actual Rufus submit control:

1. Inspect buttons inside the Rufus input/chat form.
2. Prefer a submit button with a specific Rufus id, such as `rufus-submit-button`, when present.
3. Confirm the button is `type=submit` or triggers the Rufus input form.
4. Avoid header buttons, panel controls, carousel buttons, or unrelated icons.
5. Run one test question and verify a single Rufus answer appears before queueing more questions.

Record the selector used in capture notes when possible.

## React-Safe Question Submission

Rufus is a React-driven UI. A plain DOM `el.click()` may not trigger the full event chain. Treat a submit as successful only after a new visible user-question turn appears in the Rufus conversation.

Preferred sequence for typed questions:

1. Focus the Rufus input.
2. Set the input value through a browser-realistic path, such as native input property setter plus `input`/`change` events, Playwright `fill`, or CDP keyboard events.
3. Dispatch realistic key or pointer events, such as `Enter`, `pointerdown`, `mousedown`, `mouseup`, and `click`, on the verified submit control.
4. Verify the submitted question text appears as the latest user turn.
5. If no new user turn appears, mark the attempt failed and retry with a different submit method before moving to the next question.

Do not count a question as submitted only because a click call returned without error.

Use a bounded retry budget:

- Try up to 3 submit methods for a question: form submit control, Enter key, and CDP/Playwright user-like input.
- Save `submit_attempt_count`, `submit_method`, and `failure_reason=submit_not_acknowledged` when all methods fail.
- Keep the row as `question_only` or `blocked`; do not drop it silently.

## State Machine

Automation must follow this state machine:

```text
ready
  -> login_state_check
  -> listing_snapshot
  -> product_profile
  -> question_plan
  -> submit_one_question
  -> waiting_for_thinking
  -> waiting_for_answer_stable
  -> capture_answer
  -> capture_followups
  -> ready
  -> final_save
  -> close_browser

not_logged_in
  -> blocker_save
  -> user_message_for_login
  -> close_browser

mobile_number_required
  -> blocker_save
  -> user_message_for_phone_number
  -> submit_phone_number
  -> user_message_added_phone
  -> user_message_for_sms_code
  -> submit_sms_code
  -> login_state_check
```

Never enter `submit_one_question` while a previous question is in `waiting_for_thinking` or `waiting_for_answer_stable`.

When all requested ASINs and questions are complete, save the final state and close the browser session. Preserve the browser profile or saved session only when needed for future login reuse; do not keep the browser process running in the background.

## Answer Stabilization

Consider an answer stable only when:

- no visible `Thinking...` text remains for the current question,
- no `Resume response` button is needed,
- answer text length stops changing across at least two checks,
- the captured answer belongs to the active question,
- follow-up prompts have appeared or the UI has stopped updating.

If the answer is long, such as a comparison table, use longer wait windows and capture the full raw text.

Do not wait for hard-coded strings such as `completed generating` unless they were observed in the current DOM. Use positive evidence from the current page: active answer container text, disappearance of `Thinking...`, response length stability, and no visible resume control.

Do not use phrases such as "Would you like", "Would you like me to", or similar follow-up wording as answer stop markers. These phrases can be part of a valid Rufus answer. Capture the full answer text first, then parse follow-up prompts from separate prompt buttons or clearly separated UI regions.

## Selector Strategy

Avoid selectors based on Amazon's generated or dynamic class names, such as `.rufus-papyrus-turn`, unless the selector has been verified in the current page and has a fallback. Prefer stable signals:

- element ids when present, such as `#rufus-submit-button`,
- form relationships around the Rufus input,
- roles, labels, accessible names, and button text,
- DOM position relative to the verified input and latest user question,
- text and structure checks scoped to the Rufus panel.

Before collecting many rows, run one probe question and verify that selectors can identify: input, submit control, latest user question, active answer, follow-up prompts, and `Thinking...` or resume states. If selector verification fails, save a blocker instead of starting a large capture run.

## Progress and Loop Limits

Do not expose every CDP, websocket, DOM poll, or browser probe as a user-facing progress update. Keep those checks internal and report progress only at meaningful milestones: login verified, product profile saved, question plan built, every 5 answered rows, a blocker, or final completion.

Treat progress as one of these state changes:

- a login state change,
- a completed Listing snapshot,
- a saved product profile,
- a new planned question,
- a new answered Rufus row,
- new follow-up prompts,
- a saved blocker or final report.

If three consecutive browser checks produce no state change, or if no new answered row appears within a reasonable timeout for the environment, stop the loop, save current results, and report the blocker. If the requested depth, such as 30 questions, cannot be reached because Rufus stops producing valid new questions or answers, report the exact collected count and reason.

Use per-stage timeouts instead of one long undifferentiated wait:

- submit acknowledgement timeout,
- first answer text timeout,
- answer stabilization timeout,
- follow-up prompt detection timeout,
- recovery timeout.

When a timeout occurs, retry the current stage within the bounded budget, then save the row with a specific `failure_reason`. Do not let a search/comparison answer timeout hold the whole run indefinitely.

## Stuck Conversation Recovery

If multiple submitted questions are stuck in `Thinking...`:

1. Stop submitting new questions immediately.
2. Look for a visible `Resume response` button.
3. Click `Resume response` once and wait for stabilization.
4. If recovery succeeds, mark affected row `recovery_action=resume_response`.
5. If recovery fails, mark unresolved rows `question_only` or `blocked` with `failure_reason=thinking_timeout`.
6. Reload only as a last resort; Rufus may preserve broken chat history after reload.

## Data to Capture Per Answer

Capture more than just a short summary:

- raw question,
- question origin,
- profile signal for generated questions,
- raw answer,
- answer length,
- answer type,
- follow-up prompts,
- review evidence Rufus cites,
- competitor or alternative product names,
- prices or price history ranges,
- whether Rufus recommends a different product,
- capture status and recovery action.

## Browser Cleanup

At the end of a completed capture run:

1. Confirm the latest answer rows, profile, question plan, and capture health summary have been saved.
2. Close the active browser page/session.
3. Confirm no capture-owned browser process is intentionally left running.
4. If the run ended early because of a blocker, save the blocker state before closing the browser.

## Known Rufus Answer Patterns

Rufus may answer in ways that matter for Listing optimization:

- It can directly answer feature questions with customer review support.
- It can produce long competitor comparisons with prices, ratings, and alternatives.
- It can summarize customer sentiment and mention sizing or fit complaints.
- It can assess activity fit and redirect to a more suitable product.
- It can show current price and short-term price history.

Preserve these patterns in structured fields instead of flattening everything into a generic summary.
