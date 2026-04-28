# Rufus Audit Workflow

## Audit Modes

Choose the mode that matches the user's available data:

1. **Competitor-only discovery**
   - Use when the user only provides competitor ASINs or product URLs.
   - Output buyer-concern categories and competitor strengths.
   - Do not claim the user's Listing has gaps unless the user's Listing is also provided.

2. **Own vs competitor gap analysis**
   - Use when the user provides both own and competitor products.
   - Output the full gap matrix and prioritized fixes.

3. **Retest**
   - Use when the user has an earlier Rufus baseline table.
   - Compare old vs new questions, answer tone, coverage, and unresolved risks.

## Capture Procedure

For each product page:

1. Record `marketplace`, `asin`, `brand`, `product_title`, `url`, `capture_date`, and `persona_label`.
2. Locate the Rufus panel or Rufus Q&A area.
3. Capture every visible starter question before clicking anything.
4. Click one question at a time.
5. Wait until the answer and follow-up questions stop changing.
6. Record:
   - clicked question
   - Rufus answer
   - follow-up questions
   - source depth
   - visible evidence references, if any
7. Repeat breadth-first:
   - depth 0: starter questions
   - depth 1: follow-ups after starter questions
   - depth 2: follow-ups after follow-ups
8. Stop when:
   - requested question count is reached,
   - two consecutive clicks produce no new useful questions,
   - the questions become off-topic,
   - the user-defined time limit is reached.

## Sequential Submission Rule

Never submit multiple Rufus questions concurrently. Rufus can leave later questions stacked in `Thinking...`, making answers impossible to attribute correctly.

Use this strict sequence:

1. Prepare one question.
2. Submit once.
3. Wait for `Thinking...` or loading indicators to disappear.
4. Wait until answer text is stable for at least two checks.
5. Capture raw answer, answer length, follow-up prompts, and any comparison/price/recommendation blocks.
6. Only then move to the next question.

If a submit attempt leaves several pending messages, stop the queue and recover the current conversation before sending another question.

## Capture Health Gate

Before analysis, inspect the collected rows:

1. Count rows by `capture_status`.
2. Confirm each `answered` row has both `raw_question` and `raw_answer`.
3. Confirm each `question_only`, `blocked`, or `out_of_scope` row has `failure_reason`.
4. Confirm product identity did not drift after a click: the question and answer still refer to the same ASIN or product family.
5. Confirm competitor rows and own-product rows are not mixed.

If the capture has quality problems, report them before optimization advice:

```text
Capture Health:
- Products attempted:
- Products with Rufus visible:
- Answered rows:
- Question-only rows:
- Blocked rows:
- Duplicate rows:
- Rows excluded from analysis:
- Main failure reason:
```

Do not use `question_only`, `blocked`, `duplicate`, or `out_of_scope` rows as evidence for Listing gaps. They may inform future capture tasks, but not final optimization claims.

## Recovery Procedure

When a Rufus chat gets stuck:

1. Inspect visible controls for `Resume response`.
2. Click it once and wait for answer stabilization.
3. If the answer completes, mark `recovery_action=resume_response`.
4. If page reload preserves the broken history, do not assume reload is a clean reset.
5. If no new chat/reset control is visible, continue only after the stuck answer is resolved or mark affected rows incomplete.
6. In the report, list affected question ids and do not use incomplete rows for gap claims.

## Breadth-First Collection Pattern

Use this queue logic conceptually:

```text
queue = visible starter questions
seen = normalized question text

while queue is not empty and limit not reached:
  question = queue.pop_first()
  click question
  record answer
  collect follow-up questions
  add unseen useful follow-ups to queue
```

Normalize for deduplication by lowercasing, trimming punctuation, and reducing small wording changes. Keep both questions only when the buyer intent differs.

## Result Interpretation Rules

Treat competitor Rufus answers as evidence of possible buyer concerns, not as proof that the competitor has solved the concern.

Use these distinctions:

- **Category concern**: The same issue appears across multiple competitors or is typical for the category.
- **Competitor-specific concern**: The question depends on one competitor's unique feature, bundle, material, size, or review pattern.
- **Own Listing opportunity**: The question is relevant to the user's product and controlled content can credibly answer it.
- **Unsupported gap**: The user's Listing was not provided or the capture lacks enough evidence.

Never turn a competitor-specific answer into a recommendation for the user's Listing without checking product fit.

## Observed Rufus Answer Types

Classify answer shape because each shape supports different analysis:

- `direct_answer`: Rufus gives a clear yes/no or descriptive answer, often with review evidence.
- `review_summary`: Rufus summarizes customer sentiment, fit, quality, sizing, or complaints.
- `comparison_table`: Rufus compares multiple similar products, prices, ratings, or features.
- `alternative_recommendation`: Rufus says the product may not fit and suggests another product.
- `price_history`: Rufus reports current price or historical range.
- `activity_fit`: Rufus judges whether the product is suitable for a sport, use case, venue, or rule context.

For `comparison_table`, extract competitor names, prices, ratings, and the stated reason the audited product wins or loses. For `alternative_recommendation`, record the recommended alternative and why Rufus redirected.

## Listing Comparison Procedure

Compare every normalized Rufus question against the user's available content:

1. Title
2. Bullet points
3. Main image and secondary images
4. Video
5. Product description
6. A+ content
7. FAQ or Q&A
8. Review text

Evidence quality ranking:

```text
strong controlled content > partial controlled content > review-only evidence > missing > contradicted
```

Controlled content means the seller can directly edit it: title, bullets, images, description, and A+.

## Fix Selection Rules

Choose the fix placement by question type:

- Visual proof, before/after, size, setup, parts, compatibility: image or video first.
- Trust, materials, safety, warranty, care, product limits: FAQ image, A+ FAQ, or bullets.
- Target persona and use case: lifestyle image plus A+ section.
- Comparison: objective comparison table, without naming or disparaging competitors.
- Negative review concern: usage guide, expectation-setting image, or clearer product limits.
- Core product definition: title and bullet rewrite, but avoid unnecessary broad title changes.

## Minimal Data Rules

Use these rules to decide what output is allowed:

- Competitor Q&A only: output competitor concern map and recommended next capture steps.
- Competitor Q&A plus own Listing text: output gap analysis and optimization plan.
- Own Listing only: output Listing coverage audit, but not competitor gap claims.
- Screenshots only: extract visible questions and answers, mark evidence as screenshot-derived, and note any unreadable text.
- No raw Rufus answers: output capture troubleshooting, not optimization conclusions.

## Retest Plan

Recommend retesting after meaningful Listing changes:

- Short check: 2 to 4 weeks for visible Listing coverage.
- Main check: 3 months for Rufus answer changes and new review evidence.
- Follow-up check: 6 months for trend confirmation.

Compare:

- New questions added or removed.
- Whether answers now cite or reflect improved Listing content.
- Whether high-risk concerns moved from `missing` or `weak` to `partial` or `strong`.
- Whether new negative concerns emerged from reviews.
