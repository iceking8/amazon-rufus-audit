# Rufus Audit Output Schema

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
| source_depth | yes | `0` starter, `1` follow-up, `2` deeper follow-up |
| parent_question_id | no | Link follow-up to parent question |
| raw_question | yes | Preserve original wording |
| normalized_question | yes | Cleaned and deduped wording |
| raw_answer | yes | Preserve original answer when available |
| answer_summary | yes | Short summary of Rufus answer |
| primary_category | yes | From question taxonomy |
| secondary_categories | no | Comma-separated |
| buyer_concern | yes | One-sentence concern interpretation |
| competitor_strength | no | What the competitor page seems to answer well |
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

## Report Structure

Deliver reports in this order:

1. **Executive Summary**
   - Number of products audited.
   - Number of Rufus questions captured.
   - Top 3 to 5 content gaps.
   - Biggest competitor advantage.

2. **Priority Fixes**
   - Critical and high priority rows only.
   - Include exact placement recommendations.

3. **Rufus Concern Map**
   - Group questions by category.
   - Show which competitors surface each concern.

4. **Own Listing Gap Matrix**
   - Full matrix using the coverage fields above.

5. **Optimization Plan**
   - Images: recommended image slots and callouts.
   - A+: module-level recommendations.
   - FAQ: questions and suggested answers.
   - Bullets: targeted changes.
   - Title: only conservative changes unless the user asks for a full rewrite.
   - Review strategy: review themes to encourage through legitimate post-purchase experience, not manipulation.

6. **Retest Plan**
   - What to recapture after 3 months and 6 months.
   - Which rows define success.

## CSV Header

When the user asks for a spreadsheet-ready file, use this CSV header for the baseline:

```csv
capture_id,capture_date,marketplace,product_role,asin,product_url,persona_label,source_depth,parent_question_id,raw_question,normalized_question,raw_answer,answer_summary,primary_category,secondary_categories,buyer_concern,competitor_strength,notes
```

Use this CSV header for the gap matrix:

```csv
normalized_question,primary_category,priority_score,priority_label,own_coverage_label,title_evidence,bullet_evidence,image_evidence,aplus_evidence,review_evidence,gap_type,recommended_fix_area,recommended_action,expected_rufus_benefit
```
