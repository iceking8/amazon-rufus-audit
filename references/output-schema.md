# Rufus Audit Output Schema

## Product Profile Table

Use this table before Rufus capture when Listing content is available:

| field | Required | Notes |
| --- | --- | --- |
| profile_id | yes | Stable id, such as `C1-PROFILE` |
| capture_date | yes | ISO date preferred |
| marketplace | yes | US, UK, DE, CA, etc. |
| product_role | yes | `own`, `competitor_1`, `competitor_2`, etc. |
| asin | yes | Use `unknown` if unavailable |
| product_url | yes | Source URL |
| profile_status | yes | complete, partial, blocked |
| product_type | yes | Plain-language product identity |
| target_buyer | no | Target persona or use case |
| core_use_cases | no | Main scenarios |
| key_features | no | Listing-supported feature claims |
| materials_or_specs | no | Materials, sizing, compatibility, capacity, ingredients, care, or technical specs |
| variants | no | Colors, sizes, pack counts, models, bundles, scents, etc. |
| proof_points | no | Ratings, review themes, image callouts, certifications, warranty, tests |
| likely_objections | no | Buyer concerns inferred from the Listing and category |
| product_limits | no | Limits, exclusions, non-fit use cases, or uncertainty |
| missing_listing_evidence | no | Facts that would help ask or answer Rufus questions but were not visible |
| profile_sources | yes | title, bullets, images, A+, specs, Q&A, reviews, screenshots, pasted_text |

## Question Plan Table

Use this table to show why each Rufus question is being collected:

| field | Required | Notes |
| --- | --- | --- |
| planned_question_id | yes | Stable id, such as `C1-PQ001` |
| product_role | yes | `own`, `competitor_1`, etc. |
| asin | yes | Source ASIN |
| question_text | yes | Exact question to click or submit |
| question_origin | yes | rufus_starter, rufus_followup, product_profile_generated, category_coverage_generated, user_supplied |
| profile_signal | conditional | Listing signal that caused the question; required for generated questions |
| target_category | yes | From question taxonomy |
| priority_score | yes | 1-5 |
| ask_order | yes | Recommended order |
| expected_evidence_type | no | feature, review, comparison, price, spec, use_case, limitation |
| ask_decision | yes | ask, save_only, skip_duplicate, skip_irrelevant |
| decision_reason | no | Why it was asked, saved, or skipped |

## Rufus Q&A Baseline Table

Use this table for raw and normalized capture results:

| field | Required | Notes |
| --- | --- | --- |
| capture_id | yes | Stable row id, such as `C1-Q001` |
| capture_date | yes | ISO date preferred |
| marketplace | yes | US, UK, DE, CA, etc. |
| product_role | yes | `own`, `competitor_1`, `competitor_2`, etc. |
| asin | yes | Use `unknown` if unavailable |
| product_url | yes | Source URL |
| persona_label | yes | `default` if no persona was used |
| run_environment | no | local_browser, vps_xvfb, pasted_data, screenshot, etc. |
| login_status | no | logged_in, not_logged_in, unknown |
| challenge_type | no | none, login_required, otp_or_totp_required, email_or_sms_code_required, captcha_or_robot_check, account_security_warning |
| verification_action | no | none, preauthorized_login_used, preauthorized_otp_used, human_intervention_needed |
| source_depth | yes | `0` starter, `1` follow-up, `2` deeper follow-up |
| parent_question_id | no | Link follow-up to parent question |
| clicked_path | no | Question path, such as `Q001 > Q004` |
| planned_question_id | no | Link to the question plan row when available |
| question_origin | yes | rufus_starter, rufus_followup, product_profile_generated, category_coverage_generated, user_supplied |
| profile_signal | no | Product profile signal that caused the question |
| capture_status | yes | answered, question_only, blocked, duplicate, out_of_scope |
| failure_reason | no | Required when status is not answered |
| raw_question | yes | Preserve original wording |
| normalized_question | yes | Cleaned and deduped wording |
| raw_answer | conditional | Required only when capture_status is answered |
| answer_summary | conditional | Required only when capture_status is answered |
| answer_length_chars | no | Character count of raw answer, useful for spotting unusually long comparison answers |
| answer_confidence | yes | high, medium, low, none |
| answer_type | conditional | direct_answer, review_summary, comparison_table, alternative_recommendation, price_history, activity_fit, mixed |
| follow_up_prompts | no | Follow-up prompts Rufus displayed after the answer |
| primary_category | yes | From question taxonomy |
| secondary_categories | no | Comma-separated |
| buyer_concern | yes | One-sentence concern interpretation |
| competitor_strength | no | What the competitor page seems to answer well |
| competitor_mentions | no | Competitors or alternatives Rufus mentions in the answer |
| price_evidence | no | Current price, historical range, or compared prices when Rufus reports them |
| review_evidence_summary | no | Review themes Rufus cites or paraphrases |
| concern_scope | yes | category_concern, competitor_specific, own_opportunity, unsupported_gap |
| recovery_action | no | resume_response, reload_preserved_history, none, etc. |
| notes | no | Capture caveats |

## Own Listing Coverage Matrix

Use this table after comparing against the user's Listing:

| field | Required | Notes |
| --- | --- | --- |
| normalized_question | yes | Use the question from baseline table |
| primary_category | yes | From taxonomy |
| priority_score | yes | 1-5 |
| priority_label | yes | critical, high, medium, low, watch |
| own_coverage_label | yes | strong, partial, weak, missing, contradicted, review_only, not_applicable |
| title_evidence | no | Brief evidence or `none` |
| bullet_evidence | no | Brief evidence or `none` |
| image_evidence | no | Image number and evidence, or `none` |
| aplus_evidence | no | Module and evidence, or `none` |
| review_evidence | no | Summarized review evidence, or `none` |
| gap_type | yes | From taxonomy |
| recommended_fix_area | yes | image, A+, FAQ, bullet, title, description, review strategy |
| recommended_action | yes | Specific action |
| expected_rufus_benefit | yes | What Rufus should be better able to answer |
| recommendation_confidence | yes | high, medium, low |
| evidence_needed | no | What to collect before acting if confidence is low |

## Report Structure

Deliver reports in this order:

1. **Executive Summary**
   - Number of products audited.
   - Number of Rufus questions captured.
   - Top 3 to 5 content gaps.
   - Biggest competitor advantage.

2. **Product Profile and Question Plan**
   - Product type, target buyer, core use cases, key features, likely objections, and limits.
   - Count of questions by origin.
   - Any high-priority generated questions that Rufus answered weakly or redirected.

3. **Priority Fixes**
   - Critical and high priority rows only.
   - Include exact placement recommendations.

4. **Rufus Concern Map**
   - Group questions by category.
   - Show which competitors surface each concern.

5. **Own Listing Gap Matrix**
   - Full matrix using the coverage fields above.

6. **Optimization Plan**
   - Images: recommended image slots and callouts.
   - A+: module-level recommendations.
   - FAQ: questions and suggested answers.
   - Bullets: targeted changes.
   - Title: only conservative changes unless the user asks for a full rewrite.
   - Review strategy: review themes to encourage through legitimate post-purchase experience, not manipulation.

7. **Retest Plan**
   - What to recapture after 3 months and 6 months.
   - Which rows define success.

Add **Capture Health** before Executive Summary when any row is not `answered`:

- Products attempted.
- Products with Rufus visible.
- Answered row count.
- Incomplete or blocked row count.
- Rows excluded from analysis.
- Main failure reasons.
- Whether the dataset is strong enough for gap analysis.

## CSV Header

When the user asks for a spreadsheet-ready file, use this CSV header for the baseline:

```csv
capture_id,capture_date,marketplace,product_role,asin,product_url,persona_label,source_depth,parent_question_id,raw_question,normalized_question,raw_answer,answer_summary,primary_category,secondary_categories,buyer_concern,competitor_strength,notes
```

Use this CSV header for the gap matrix:

```csv
normalized_question,primary_category,priority_score,priority_label,own_coverage_label,title_evidence,bullet_evidence,image_evidence,aplus_evidence,review_evidence,gap_type,recommended_fix_area,recommended_action,expected_rufus_benefit
```

For V2 capture files, prefer this expanded baseline header:

```csv
capture_id,capture_date,marketplace,product_role,asin,product_url,persona_label,run_environment,login_status,challenge_type,verification_action,source_depth,parent_question_id,clicked_path,planned_question_id,question_origin,profile_signal,capture_status,failure_reason,raw_question,normalized_question,raw_answer,answer_summary,answer_confidence,primary_category,secondary_categories,buyer_concern,competitor_strength,concern_scope,notes
```

For automation-heavy captures, use this full forensic header:

```csv
capture_id,capture_date,marketplace,product_role,asin,product_url,persona_label,run_environment,login_status,challenge_type,verification_action,source_depth,parent_question_id,clicked_path,planned_question_id,question_origin,profile_signal,capture_status,failure_reason,raw_question,normalized_question,raw_answer,answer_summary,answer_length_chars,answer_confidence,answer_type,follow_up_prompts,primary_category,secondary_categories,buyer_concern,competitor_strength,competitor_mentions,price_evidence,review_evidence_summary,concern_scope,recovery_action,notes
```
