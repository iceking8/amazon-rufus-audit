---
name: amazon-rufus-audit
description: Amazon Rufus competitor Q&A audit for sellers. Use when Codex needs to collect or normalize Rufus questions and answers from competitor or own Amazon product pages, classify buyer concerns, compare those questions against the user's Listing content, find weak or missing answers, and produce prioritized image, A+, FAQ, bullet, and title optimization recommendations. Trigger on requests mentioning Rufus 问答采集, Rufus 拆解, 竞品 Rufus 分析, Listing gap analysis, AI-ready Listing, Amazon Rufus audit, or comparing competitor Rufus answers with the user's product link.
---

# Amazon Rufus Audit

## Overview

Use this skill to turn Rufus Q&A from competitor and own Amazon product pages into a repeatable Listing optimization workflow. The goal is not to manipulate Rufus; the goal is to discover buyer concerns Rufus can see, compare content coverage, and decide what to strengthen in the user's Listing.

## Core Principle

Treat Rufus Q&A as buyer-intent evidence. A useful audit answers three questions:

1. What questions does Rufus surface on competitor pages?
2. Does the user's Listing answer the same concerns clearly and credibly?
3. Which missing or weak answers should be fixed in images, A+, FAQ, bullets, title, or review-generation strategy?

Do not promise ranking control, Rufus manipulation, or guaranteed recommendation outcomes. Phrase recommendations as content coverage and conversion-risk improvements.

## Inputs

Ask for only the missing information needed for the requested depth:

- User's product URL or ASIN.
- Competitor product URLs or ASINs.
- Marketplace, such as US, UK, DE, CA.
- Product category and target buyer if not obvious.
- Existing Listing content, screenshots, or exported page text when browser access is unavailable.
- Optional persona labels for analysis, such as "parents with young kids", "minimalist home office", or "price-sensitive buyer".

If a task requires typing persona details into Amazon profile features such as "Tell us about you", stop and ask for explicit confirmation before entering that data. Prefer labeling personas in the audit table without changing the user's Amazon account profile.

## Workflow

1. Establish the audit set.
   - Identify the user's product as `own`.
   - Identify each competitor as `competitor_1`, `competitor_2`, etc.
   - Record marketplace, ASIN, URL, capture date, and persona label.

2. Collect Rufus Q&A.
   - Open each product page and locate the Rufus panel or Rufus Q&A area.
   - Capture the visible starter questions.
   - Click each relevant question, wait for the answer to stabilize, then record the question and answer.
   - Collect new follow-up questions generated after each answer.
   - Continue breadth-first until the user-requested depth is reached, the panel stops producing useful new questions, or the audit has enough coverage.
   - Deduplicate near-identical questions, but keep materially different wording when it reveals a different buyer concern.

3. Normalize the dataset.
   - Use [references/output-schema.md](references/output-schema.md) for table fields.
   - Use [references/question-taxonomy.md](references/question-taxonomy.md) for category, priority, and issue labels.
   - Preserve original question and answer text before summarizing.

4. Compare against the user's Listing.
   - Check title, bullets, images, video, description, A+, FAQ, review text, and Q&A if available.
   - Mark coverage as `strong`, `partial`, `weak`, `missing`, or `contradicted`.
   - Prefer concrete evidence: quote or summarize where the Listing answers the question.
   - Distinguish Listing evidence from review-only evidence; review-only answers are useful but usually weaker than controlled Listing content.

5. Prioritize fixes.
   - Prioritize questions tied to conversion risk, negative reviews, safety/material concerns, compatibility, sizing, usage friction, target persona fit, and direct competitor comparison.
   - Recommend the smallest effective Listing change first.
   - For title changes, be conservative; suggest incremental edits and explain the risk of broad title rewrites.

6. Produce the audit report.
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

When browser access is unavailable, accept pasted Rufus Q&A, screenshots, HTML exports, or user-made spreadsheets and continue from normalization and analysis.

## Output Standards

Use the detailed schemas in [references/output-schema.md](references/output-schema.md). At minimum, produce:

- Rufus Q&A baseline table.
- Competitor concern map.
- Own Listing coverage matrix.
- Prioritized gap list.
- Specific Listing optimization plan.
- Retest plan.

Make recommendations practical and placement-specific. Prefer "add an FAQ image answering X with these 3 callouts" over "improve content".

## Reference Files

- [references/audit-workflow.md](references/audit-workflow.md): Detailed capture, comparison, and retest workflow.
- [references/question-taxonomy.md](references/question-taxonomy.md): Question categories, priority scoring, and coverage labels.
- [references/output-schema.md](references/output-schema.md): Tables and report structure to use in deliverables.
