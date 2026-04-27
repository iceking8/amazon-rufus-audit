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
