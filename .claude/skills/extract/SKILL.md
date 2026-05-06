---
name: extract
description: Run field-level extraction from ingested document chunks. Populates the structured project schema. Tags each field with confidence level and source evidence.
---

# /extract — Field-Level Structured Extraction

## When to Use

- After `/ingest` has processed documents
- User says "extract my project data" or "analyze my docs"
- Auto-triggered by `/quick-start` after ingestion

## Inputs

- Ingested document chunks from `/ingest`
- Schema structure from `templates/project-schema.json`
- Extraction prompts from `prompts/extraction-prompts.md`

## Process

### Step 1: Load Schema Target

Reference `templates/project-schema.json` as the extraction target. You are populating each field in this schema.

### Step 2: Run Per-Field Extraction

For each field in the schema, run a focused extraction pass across all document chunks.

**Critical rule: Only extract what is explicitly stated. Do not infer or fill gaps.**

For each field, produce:
```json
{
  "value": "extracted content — direct quote or close paraphrase",
  "confidence": "high | medium | low | none",
  "evidence": ["supporting quote 1", "supporting quote 2"],
  "needs_user_input": true | false
}
```

Confidence definitions:
- `high` — explicitly stated with specifics (real numbers, named people, clear ownership language)
- `medium` — present but vague or implied
- `low` — can be reasonably inferred but not stated
- `none` — no basis in source documents

Set `needs_user_input: true` when:
- Confidence is `low` or `none`
- The field is high-priority (role ownership, outcomes, key decision, problem statement)

### Step 3: High-Priority Fields

Give extra attention to these — they are the highest signal for PM evaluation and case study quality:

**role_scope.what_i_owned**
Look for first-person ownership language: "I owned," "I led," "I was responsible for," "my area was." Distinguish clearly between owned, contributed, influenced, and supported. Flag vague language like "we built" or "the team shipped" — these cannot be attributed to the user without clarification.

**problem.problem_statement**
Extract who had the problem, what state things were in before, and why fixing it mattered to the business. Also extract 3-4 distinct problem dimensions — named, discrete aspects of the problem that can each be labeled with a short noun phrase. If the problem is described only at a surface level, mark `medium` and flag for gap detection.

**outcomes**
Extract any quantitative results first. If none are present, extract validation signals, directional signals, or qualitative evidence. If neither exists, mark `none` — do not invent placeholder outcomes.

**strategy_decisions.named_decisions**
Extract individual named decisions as discrete items: decision name, options considered, choice made, rationale, tradeoff accepted. A decision without alternatives is lower value — flag it. If 4+ named decisions with alternatives exist, mark `table_viable: true`.

**strategy_decisions.tradeoffs**
Look for language showing options considered and rejected. If only the chosen direction is described with no alternatives mentioned, mark `low` and flag.

**discovery.top_insights**
Extract actual findings that changed something — not just methods used. "We ran 20 interviews" is a method. "Users were dropping because of X, not Y as we assumed" is an insight. For each insight, extract a short memorable label (the kind that could become a section headline) plus the explanation.

**pull_quotes**
Extract any verbatim user quotes, customer verbatims, field observations, or memorable PM statements. These ground the case study in a human moment and are used as pull quote blocks. Only extract verbatim or near-verbatim language — do not paraphrase into pull quote form.

**solution.breakdown_principle**
Extract the structural principle that most naturally organizes the solution: by user type (different users have different experiences), by feature or UX decision (key choices were interface or workflow decisions), or by pipeline component (system with distinct stages). This determines how the Solution section is formatted.

### Step 4: Merge Across Chunks

When the same field appears in multiple chunks, merge the strongest evidence. Prioritize:
- Specificity over vagueness
- Hard numbers over approximations
- Explicit first-person ownership over team-level language

### Step 5: Write to Context Library

Write extracted values to `context-library/project-inputs-[slug].md`, where `[slug]` is the project slug derived during `/ingest`. Fill in only the fields that have evidence — leave placeholders for fields with `none` confidence.

If the slug is not known (e.g., `/extract` was run standalone without a prior `/ingest`), ask: "What's the project name? I'll use it to name the input file."

### Step 6: Extraction Summary

Tell the user:
- Which fields extracted with high confidence
- Which fields need clarification (will be handled in `/gap-detect`)
- Any conflicting signals found across documents

Example:
"Extraction complete.
Strong data on: problem statement, user definition, solution, execution milestones.
Needs clarification: outcomes (no specific metrics found), role ownership (team language used throughout), key strategic decision.
Proceeding to gap detection..."

### Step 7: Auto-Proceed

Automatically continue to `/gap-detect` unless the user asks to stop.

## Rules

- Extract only what is explicitly in the source. Mark gaps honestly.
- Preserve exact numbers, percentages, dates, and quotes from source documents.
- If the same data point appears with different values across documents, surface the conflict: "Your PRD says X, but your resume bullet says Y — which is correct?"
- Do not output the full schema to the user — give a summary, not a data dump.
