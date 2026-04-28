# Product Profiling and Question Planning

Use this reference before fresh Rufus capture. The purpose is to avoid generic question harvesting: first understand what the product is, then ask Rufus questions that match the product's real buyer concerns.

## Product Profiling Gate

Before submitting custom Rufus questions, read the Listing evidence available on the product page or in user-provided exports:

- title and brand,
- price, coupon, rating, review count, and variant options,
- bullets and product details,
- main image and secondary image callouts,
- video, A+ content, product description, and comparison charts when visible,
- existing Amazon Q&A,
- review themes, especially repeated praise, complaints, sizing, defects, and use-case surprises.

If some evidence is unavailable, record it as `not_captured` instead of guessing.

## Product Profile Fields

Create a compact product profile for each ASIN:

| field | Meaning |
| --- | --- |
| product_type | Plain-language product identity |
| target_buyer | Main buyer persona or use case |
| core_use_cases | Where, when, and why buyers use it |
| key_features | Features the Listing emphasizes |
| materials_or_specs | Materials, size, capacity, technical specs, ingredients, or care details |
| variants | Size, color, pack count, model, scent, or bundle differences |
| proof_points | Evidence such as reviews, ratings, certifications, warranties, tests, or image claims |
| likely_objections | Concerns buyers may ask before purchasing |
| product_limits | What the product may not do well, should not be used for, or may not fit |
| missing_listing_evidence | Important facts not visible in controlled Listing content |

## Question Plan Construction

Build a question plan from five sources:

1. `rufus_starter`: visible Rufus starter questions.
2. `rufus_followup`: questions Rufus displays after an answer.
3. `product_profile_generated`: questions generated from the product profile.
4. `category_coverage_generated`: category-standard questions used to fill coverage gaps.
5. `user_supplied`: questions the user explicitly wants tested.

For each planned question, record:

- question text,
- question origin,
- profile signal that caused it,
- target category,
- priority,
- expected evidence type,
- whether it should be asked before or after Rufus starter questions.

## Product-Specific Question Heuristics

Generate questions that match the product type instead of relying on a universal checklist:

- Apparel: stretch, fit, sheerness, washing, pockets, lining, occasion, sport/activity fit, size complaints.
- Electronics: compatibility, battery life, setup, charging, app requirements, warranty, failure modes.
- Home goods: dimensions, materials, installation, cleaning, space fit, durability, included parts.
- Baby, pet, and safety products: age or weight limits, materials, toxicity, adult ease of use, failure risk, certification.
- Beauty and health-adjacent products: ingredients, allergens, scent, texture, routine fit, claim-risk wording.
- Tools and outdoor products: strength, weather resistance, capacity, assembly, portability, replacement parts.
- Consumables: flavor, serving size, storage, shelf life, dietary fit, recurring complaints.

Use these as prompts, not rigid templates. If a question does not match the product profile, do not ask it just to fill a category.

## Coverage Ladder

A strong collection blends:

- Rufus visible questions, because they show what Rufus already surfaces.
- Product-profile questions, because they test the buyer concerns most relevant to this ASIN.
- Category-coverage questions, because they prevent obvious blind spots.
- Comparison questions, because Rufus may reveal alternatives, prices, ratings, and redirect risks.
- Review-summary questions, because Rufus often uses customer language that the Listing does not say explicitly.

## When Only Competitor Links Are Provided

Still build a profile for each competitor. Use it to explain why Rufus may surface certain questions and which concerns are category-level versus competitor-specific.

Do not convert competitor-specific traits into recommendations for the user's Listing until the user's product profile is available.

## Minimum Product-Aware Question Set

For each product, aim to test at least:

- one feature confirmation question,
- one use-case or persona question,
- one sizing, compatibility, or spec question when relevant,
- one setup, care, or maintenance question when relevant,
- one customer sentiment question,
- one negative-review or limitation question,
- one comparison or alternative question,
- one price/value question when Rufus supports it.

Skip irrelevant slots and state why they were skipped.
