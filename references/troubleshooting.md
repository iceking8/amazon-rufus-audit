# Rufus Capture Troubleshooting

Use this reference when a Rufus capture run returns incomplete rows, missing answers, duplicates, or confusing recommendations.

## Common Failure Modes

| symptom | Likely cause | What to record |
| --- | --- | --- |
| Rufus panel is not visible | Marketplace, account, browser, region, or A/B availability issue | `capture_status=blocked`, `failure_reason=rufus_not_visible` |
| Question is visible but answer never appears | Loading failure, network issue, panel state issue | `capture_status=question_only`, keep raw question |
| Several later questions are stuck on `Thinking...` | Questions were submitted concurrently or before the previous answer stabilized | Stop queue, try `Resume response`, mark unresolved rows incomplete |
| Same question repeats many times | Rufus follow-up loop or duplicate starter questions | `capture_status=duplicate`, link to kept row in notes |
| Answer talks about a different item | Sponsored module, comparison drift, or page state drift | `capture_status=out_of_scope` |
| Answer is generic category advice | Rufus did not ground the answer in the current ASIN | `answer_confidence=low`, `concern_scope=category_concern` |
| Browser asks for login, CAPTCHA, or permission | Access barrier | Stop and ask the user before solving CAPTCHA or logging in if not already approved |
| Persona settings are needed | Amazon profile change would transmit persona data | Ask for explicit confirmation before typing profile/persona details |

## Capture Repair Checklist

When capture quality is poor:

1. Retry the same ASIN in a fresh page.
2. Capture starter questions before clicking any question.
3. Click one question, wait for the answer to stop changing, then copy the answer.
4. Record the clicked path before moving to follow-ups.
5. Stop a branch after two repeated or off-topic follow-ups.
6. Keep failed rows instead of deleting them; failed rows explain why analysis may be limited.

## Automation Guardrails

- Identify the real Rufus submit button. A known good selector can be `#rufus-submit-button`, but still verify it is inside the Rufus input form and has submit behavior.
- Do not click header buttons or generic buttons near the Rufus panel unless their role is confirmed.
- Submit only one question at a time.
- Treat `Thinking...` as an active generation state. Do not send another question while any prior row is still thinking.
- Page reload may preserve Rufus conversation history, including broken pending responses.
- If a visible `Resume response` button appears, use it as a recovery action and record it in the row.
- If no clean reset or new chat exists, do not pretend the next capture is fresh; record the retained history in notes.

## Analysis Safety Rules

- Do not infer a Rufus answer from product bullets, reviews, or images when no answer was captured.
- Do not use blocked, duplicate, or out-of-scope rows as evidence for Listing changes.
- Do not claim "your Listing is missing X" unless the user's Listing content was provided or inspected.
- Do not recommend adding a competitor-specific feature unless the user's product actually has that feature.
- Do not turn review themes into claims unless the claim can be supported objectively and compliantly.
- Do not reduce a long comparison answer to "good value" only; preserve competitor names, prices, ratings, and why Rufus preferred or rejected the audited product.

## Suggested Capture Health Language

Use plain language in the final report:

```text
This capture is useful for buyer-concern discovery, but it is not strong enough for a full gap analysis yet because 8 of 21 rows have questions without Rufus answers. I will use answered rows for insight and list the incomplete rows separately for recapture.
```
