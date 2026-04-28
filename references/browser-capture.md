# Browser Capture Guidance

Use this reference when automating Rufus Q&A capture through a browser, CDP, Playwright, or another UI automation layer.

For login-required capture, also follow [account-safety.md](account-safety.md).

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

## State Machine

Automation must follow this state machine:

```text
ready
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
