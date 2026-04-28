# Rufus Question Taxonomy

## Question Categories

Assign one primary category and optional secondary categories.

| category | Use for |
| --- | --- |
| use_case | Where, when, or why the product is used |
| target_persona | Buyer identity, lifestyle, family, age, pet, hobby, or professional role |
| compatibility | Fit with devices, spaces, accessories, sizes, or existing routines |
| size_fit | Measurements, fit, capacity, dimensions, sizing uncertainty |
| material_safety | Materials, ingredients, safety, allergens, toxicity, child/pet use |
| durability_quality | Durability, lifespan, construction, defect risk |
| setup_usage | Installation, assembly, care, maintenance, learning curve |
| care_wash | Washing, cleaning, drying, care instructions, maintenance details |
| performance_result | Expected outcome, before/after, speed, strength, effectiveness |
| customer_sentiment | What customers say, review summaries, common praise or complaints |
| package_accessory | What is included, variants, missing parts, packaging |
| comparison | Product vs competitor, variant vs variant, alternative choices |
| alternative_recommendation | Rufus recommends another product or says this one is not ideal |
| price_history | Current price, price range, discount history, value over time |
| sport_activity_fit | Whether the product is suitable for a specific sport, activity, dress code, or venue |
| negative_review | Concerns surfaced from complaints or low-star review themes |
| price_value | Whether the product is worth it, value perception, budget fit |
| warranty_support | Warranty, returns, customer support, replacement |
| compliance_claim | Regulated or risky claims such as medical, environmental, or safety claims |
| unknown | Useful question that does not fit another category |

## Coverage Labels

Use one coverage label for the user's Listing:

| label | Meaning |
| --- | --- |
| strong | The Listing answers clearly in controlled content with concrete evidence |
| partial | The Listing answers, but the answer is incomplete, vague, buried, or only in one weak placement |
| weak | The Listing hints at the answer but a buyer or Rufus could still be uncertain |
| missing | No usable answer is found in Listing content or reviews |
| contradicted | Listing claims conflict with reviews, Q&A, or other evidence |
| review_only | Reviews answer the question, but controlled Listing content does not |
| not_applicable | The question does not apply to the user's product |

## Capture Status Labels

Use one capture status for every collected row:

| label | Meaning |
| --- | --- |
| answered | Question and Rufus answer were captured successfully |
| question_only | Question was captured, but no Rufus answer was captured |
| blocked | Capture was blocked by missing Rufus panel, login wall, CAPTCHA, region issue, or page load failure |
| duplicate | Question is a near duplicate of a stronger existing row |
| out_of_scope | Question or answer refers to another product, sponsored content, general category advice, or non-buyer intent |

## Question Origin Labels

Use one origin for every question:

| label | Meaning |
| --- | --- |
| rufus_starter | Rufus displayed the question before any interaction |
| rufus_followup | Rufus displayed the question after a prior answer |
| product_profile_generated | The agent generated the question from observed Listing/product signals |
| category_coverage_generated | The agent generated the question from category-standard coverage checks |
| user_supplied | The user explicitly asked to test this question |

Generated questions must include a `profile_signal` unless the origin is `category_coverage_generated` and the question is clearly category-standard.

## Answer Type Labels

Use one answer type for every answered row:

| label | Meaning |
| --- | --- |
| direct_answer | Direct answer to a specific product question |
| review_summary | Summary of customer reviews or sentiment |
| comparison_table | Comparison against similar products, competitors, prices, or ratings |
| alternative_recommendation | Rufus recommends a different product or explains why this product is not ideal |
| price_history | Current or historical price information |
| activity_fit | Suitability for an activity, sport, venue, rule context, or occasion |
| mixed | More than one answer shape is materially present |

## Answer Confidence Labels

Use one confidence label for every answer summary:

| label | Meaning |
| --- | --- |
| high | Full answer is visible and product identity is clear |
| medium | Answer is visible but may be truncated, generalized, or missing minor context |
| low | Answer is partial, screenshot text is hard to read, or product identity is uncertain |
| none | No answer was captured |

## Priority Scoring

Score each question from 1 to 5.

Start at 1, then add points:

- +1 if it affects purchase confidence or conversion.
- +1 if it is directly tied to a product profile signal, such as a claimed feature, material, use case, variant, or likely objection.
- +1 if it appears on multiple competitor pages or repeats in different wording.
- +1 if it relates to negative reviews, durability, safety, compatibility, sizing, or product limits.
- +1 if the user's coverage is `missing`, `weak`, `review_only`, or `contradicted`.
- +1 if the fix is feasible in controlled content within one Listing update.

Cap at 5.

Priority interpretation:

| score | label | Meaning |
| --- | --- | --- |
| 5 | critical | Fix in the next Listing update |
| 4 | high | Fix soon; likely conversion impact |
| 3 | medium | Useful improvement, schedule after critical/high items |
| 2 | low | Monitor or handle if easy |
| 1 | watch | Keep as context; no immediate change |

## Gap Types

Use these issue labels in the report:

- `missing_answer`: Competitors answer the concern but the user's Listing does not.
- `weak_answer`: User answers, but the answer is vague or less persuasive than competitors.
- `buried_answer`: User answers in a low-visibility place, but not in images/A+ or bullets.
- `review_risk`: Reviews create concern that controlled content does not address.
- `contradiction`: Controlled content and buyer evidence conflict.
- `persona_gap`: User does not speak to a buyer identity or use case competitors cover.
- `comparison_gap`: User lacks objective comparison points for recurring Rufus comparison questions.
- `visual_gap`: The best answer should be visual but no image/video covers it.
- `claim_risk`: Proposed answer could create compliance risk and needs careful wording.
- `capture_gap`: The capture is incomplete, so no Listing conclusion should be made yet.
- `product_fit_unclear`: Competitor concern may not apply to the user's product without more product details.
- `rufus_redirect_risk`: Rufus recommends an alternative product because the audited product is weak for the asked use case.
- `review_signal_gap`: Rufus relies on customer review language that the Listing does not make explicit in controlled content.

## Recommendation Tone

Be practical and evidence-based:

- Prefer objective attributes over hype.
- Avoid disparaging competitors by name.
- Avoid unsupported medical, environmental, safety, or absolute claims.
- Give exact placement advice, such as image 6 FAQ, A+ Module 3 comparison table, or bullet 4 care instructions.
