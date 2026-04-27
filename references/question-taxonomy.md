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
| performance_result | Expected outcome, before/after, speed, strength, effectiveness |
| package_accessory | What is included, variants, missing parts, packaging |
| comparison | Product vs competitor, variant vs variant, alternative choices |
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

## Priority Scoring

Score each question from 1 to 5.

Start at 1, then add points:

- +1 if it affects purchase confidence or conversion.
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

## Recommendation Tone

Be practical and evidence-based:

- Prefer objective attributes over hype.
- Avoid disparaging competitors by name.
- Avoid unsupported medical, environmental, safety, or absolute claims.
- Give exact placement advice, such as image 6 FAQ, A+ Module 3 comparison table, or bullet 4 care instructions.
