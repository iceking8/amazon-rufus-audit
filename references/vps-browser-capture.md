# VPS/Xvfb Browser Capture

Use this reference only after the agent has already identified that Rufus capture is running on a VPS or virtual-display browser runtime such as Linux + Chrome + Xvfb. This file is optional; do not require environment detection before every capture run.

## Runtime Notes

- Use one persistent Chrome profile for the authorized buyer account unless the user requests a clean login.
- Keep one active Amazon browser session for collection; do not run multiple parallel Amazon tabs or workers.
- Save per-ASIN state files and screenshots or HTML snippets when the user wants evidence.
- Let the agent choose the practical browser launch details for the current OS and runtime.

## Login and Rufus Visibility Gate

Rufus may require a logged-in Amazon buyer account.

1. Check whether the user supplied a pre-authorized login workflow.
2. If a workflow exists, follow it without printing secrets or one-time codes.
3. If login succeeds, confirm account state only at a high level, such as `logged_in=true`; never expose the account email, cookies, OTP seed, or session tokens.
4. Open the product page and confirm Rufus is visible before starting product profiling or custom questions.
5. If Rufus is not visible, classify the reason when possible: `not_logged_in`, `marketplace_unavailable`, `page_load_failed`, `asin_no_rufus`, `account_not_eligible`, or `unknown`.

## Single-ASIN Run Pattern

Use a low-frequency, UI-driven pattern:

1. Process one ASIN at a time.
2. Take a Listing snapshot.
3. Build the product profile.
4. Create the question plan.
5. Save visible Rufus starter questions.
6. Ask one question and wait for the answer to stabilize.
7. Save the result immediately.
8. Continue only after the UI is ready.
9. Finish the ASIN, write a run summary, then cool down before the next ASIN.

Do not use parallel ASIN collection, hidden Amazon endpoints, proxy rotation, fingerprint spoofing, or CAPTCHA-bypass tooling.

## Human-Paced Stability

Human-paced means conservative UI timing for stability, not an attempt to bypass platform controls:

- Wait for page content to load before scrolling or clicking.
- Read and save visible Listing sections before opening Rufus.
- Scroll only as needed to expose page content relevant to profiling.
- Use short pauses between UI actions and longer pauses after long Rufus answers.
- Stop after repeated page failures, repeated `Thinking...` timeouts, or account/security prompts.

## Resumable State

Write progress after every meaningful step:

- login status and verification action, without secrets,
- ASIN and URL,
- profile status,
- planned question queue,
- completed question ids,
- failed question ids and reasons,
- recovery actions,
- capture health counts.

On restart, resume from the last stable state. Do not resubmit already answered questions unless the user explicitly requests a retest.

## Stop Conditions

Pause the run and report clearly when:

- Amazon shows account security warnings,
- Rufus disappears after login,
- a challenge appears and no pre-authorized workflow exists,
- the page repeatedly fails to load,
- several answers remain stuck after recovery,
- the collected data is too incomplete for gap analysis.
