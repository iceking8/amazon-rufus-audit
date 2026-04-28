---
name: amazon-rufus-audit
description: Amazon Rufus competitor Q&A audit for sellers. Use when Codex needs to read Amazon Listing content, build a product profile, generate product-aware Rufus question plans, collect, clean, validate, or analyze Rufus questions and answers from competitor or own product pages; diagnose capture problems; classify buyer concerns; compare those questions against the user's Listing content; find weak or missing answers; and produce prioritized image, A+, FAQ, bullet, and title optimization recommendations. Trigger on requests mentioning Rufus 问答采集, Rufus 拆解, 竞品 Rufus 分析, Listing gap analysis, AI-ready Listing, Amazon Rufus audit, Rufus capture troubleshooting, or comparing competitor Rufus answers with the user's product link.
---

# Amazon Rufus Audit

## Overview

Use this skill to turn Rufus Q&A from competitor and own Amazon product pages into a repeatable Listing optimization workflow. The goal is not to manipulate Rufus; the goal is to discover buyer concerns Rufus can see, compare content coverage, and decide what to strengthen in the user's Listing.

For every run, separate the work into three phases: product profiling first, capture validation second, Listing gap analysis third. Do not collect only generic questions when product-specific Listing signals are available. Do not perform gap analysis on incomplete captures unless the gaps are clearly supported by available evidence.

## Core Principle

Treat Rufus Q&A as buyer-intent evidence. A useful audit answers four questions:

1. What questions does Rufus surface on competitor pages?
2. What product-specific questions should be asked based on the Listing's title, bullets, images, A+, reviews, specifications, and category?
3. Does the user's Listing answer the same concerns clearly and credibly?
4. Which missing or weak answers should be fixed in images, A+, FAQ, bullets, title, or review-generation strategy?

Do not promise ranking control, Rufus manipulation, or guaranteed recommendation outcomes. Phrase recommendations as content coverage and conversion-risk improvements.

## Inputs

Ask for only the missing information needed for the requested depth:

- User's product URL or ASIN.
- Competitor product URLs or ASINs.
- Marketplace, such as US, UK, DE, CA.
- Product category and target buyer if not obvious.
- Existing Listing content, screenshots, or exported page text when browser access is unavailable.
- Optional persona labels for analysis, such as "parents with young kids", "minimalist home office", or "price-sensitive buyer".
- Optional pre-authorized Amazon login and OTP/TOTP workflow, supplied outside the skill through secrets or environment variables.

If a task requires Amazon login, first check whether the user supplied a pre-authorized login and verification workflow. Use that workflow only for the user's own account and never print credentials, cookies, OTP seeds, or one-time codes. If no workflow is available, ask for human help. If a task requires typing persona details into Amazon profile features such as "Tell us about you", stop and ask for explicit confirmation before entering that data. Prefer labeling personas in the audit table without changing the user's Amazon account profile.

## Workflow

1. Establish the audit set.
   - Identify the user's product as `own`.
   - Identify each competitor as `competitor_1`, `competitor_2`, etc.
   - Record marketplace, ASIN, URL, capture date, and persona label.
   - For automated browser collection, use a single account, process one ASIN at a time, keep one active browser session, and save resumable state.

2. Build product profile and collect Rufus Q&A.
   - Before submitting custom questions, read visible Listing content and build a product profile: title, brand, price, rating, bullets, product details, variant options, image callouts, A+ content, Q&A, and review themes when available.
   - Create a product-aware question plan that combines Rufus visible starter questions with generated questions based on product features, use cases, materials, sizing, compatibility, care, limits, and likely buyer objections.
   - Mark each planned question with `question_origin`: `rufus_starter`, `rufus_followup`, `product_profile_generated`, `category_coverage_generated`, or `user_supplied`.
   - See [references/product-profiling.md](references/product-profiling.md) for the required profiling gate and question-plan structure.
   - Open each product page and locate the Rufus panel or Rufus Q&A area.
   - Capture the visible starter questions.
   - Ask product-profile-generated questions only after starter questions are saved, unless the user specifically asks for a targeted capture.
   - Submit or click exactly one question at a time, wait for the answer to stabilize, then record the question and answer.
   - If automating the Rufus chat input, use the actual Rufus submit control, not nearby header buttons. See [references/browser-capture.md](references/browser-capture.md).
   - Collect new follow-up questions generated after each answer.
   - Continue breadth-first until the user-requested depth is reached, the panel stops producing useful new questions, or the audit has enough coverage.
   - Deduplicate near-identical questions, but keep materially different wording when it reveals a different buyer concern.

3. Validate capture quality.
   - Every row must have a `capture_status`: `answered`, `question_only`, `blocked`, `out_of_scope`, or `duplicate`.
   - Every answered row must keep raw question text and raw answer text.
   - Every incomplete row must include `failure_reason`, such as Rufus not visible, answer did not load, CAPTCHA/login wall, page mismatch, repeated question, or off-topic recommendation.
   - Any row stuck on `Thinking...` must be recovered or marked incomplete; do not continue submitting new questions into a stuck conversation.
   - If more than 30% of rows are incomplete, output a Capture Health section before any analysis.
   - If product profiling could not be completed, state what Listing evidence was unavailable and avoid over-targeted conclusions.
   - If only competitor data is available, produce competitor insight and question taxonomy, but do not claim the user's Listing has gaps.

4. Normalize the dataset.
   - Use [references/output-schema.md](references/output-schema.md) for table fields.
   - Use [references/question-taxonomy.md](references/question-taxonomy.md) for category, priority, and issue labels.
   - Preserve original question and answer text before summarizing.

5. Compare against the user's Listing.
   - Check title, bullets, images, video, description, A+, FAQ, review text, and Q&A if available.
   - Mark coverage as `strong`, `partial`, `weak`, `missing`, or `contradicted`.
   - Prefer concrete evidence: quote or summarize where the Listing answers the question.
   - Distinguish Listing evidence from review-only evidence; review-only answers are useful but usually weaker than controlled Listing content.

6. Prioritize fixes.
   - Prioritize questions tied to conversion risk, negative reviews, safety/material concerns, compatibility, sizing, usage friction, target persona fit, and direct competitor comparison.
   - Recommend the smallest effective Listing change first.
   - For title changes, be conservative; suggest incremental edits and explain the risk of broad title rewrites.

7. Produce the audit report.
   - Start with capture health if there were missing answers, blocked pages, duplicates, or unsupported conclusions.
   - Include the product profile and question-plan logic when the task involves fresh collection or troubleshooting.
   - Start with the top priority gaps.
   - Then provide the full Rufus Q&A matrix.
   - Then provide concrete optimization actions by placement: images, A+, FAQ, bullets, title, description, review strategy.
   - Include a retest plan, usually 3 months and 6 months after material Listing changes.

## Capture Guidance

Use browser-visible content as the source of truth. Amazon page structures change often, so do not rely on brittle selectors unless the user specifically asks for automation code.

When collecting manually or semi-manually:

- Screenshot or copy visible Rufus questions before clicking.
- Record the clicked path when a question appears only after another question.
- Keep competitor and own-product captures separate.
- Avoid logging into or changing Amazon account settings unless the user explicitly asks and confirms the specific action.
- If Rufus is unavailable or hidden on a page, record a blocked row and continue with the next product instead of fabricating questions.
- If a question appears but no answer is captured, keep it as `question_only`; do not summarize an answer from surrounding page content.
- If the answer appears to reference a different product, sponsored item, or general category advice, mark it `out_of_scope`.
- If a previous response is stuck, look for a visible `Resume response` control. If it completes, keep the row with a recovery note; if it does not, mark the affected rows `question_only` or `blocked`.

When browser access is unavailable, accept pasted Rufus Q&A, screenshots, HTML exports, or user-made spreadsheets and continue from normalization and analysis.

## Output Standards

Use the detailed schemas in [references/output-schema.md](references/output-schema.md). At minimum, produce:

- Capture health summary.
- Product profile summary.
- Question plan with question origins.
- Rufus Q&A baseline table.
- Competitor concern map.
- Own Listing coverage matrix.
- Prioritized gap list.
- Specific Listing optimization plan.
- Retest plan.

Make recommendations practical and placement-specific. Prefer "add an FAQ image answering X with these 3 callouts" over "improve content".

## Reference Files

- [references/audit-workflow.md](references/audit-workflow.md): Detailed capture, comparison, and retest workflow.
- [references/product-profiling.md](references/product-profiling.md): Required Listing-read gate and product-aware question planning.
- [references/browser-capture.md](references/browser-capture.md): Browser automation guardrails for Rufus chat collection.
- [references/vps-browser-capture.md](references/vps-browser-capture.md): Optional notes for low-frequency VPS/Xvfb runs after the agent has already identified that runtime.
- [references/account-safety.md](references/account-safety.md): Pre-authorized login, OTP/TOTP handling, secret hygiene, and human-intervention rules.
- [references/question-taxonomy.md](references/question-taxonomy.md): Question categories, priority scoring, and coverage labels.
- [references/output-schema.md](references/output-schema.md): Tables and report structure to use in deliverables.
- [references/troubleshooting.md](references/troubleshooting.md): Rufus capture failure modes and how to report them.
