---
name: extract
description: Runs field-level extraction from ingested PM project document chunks and populates the structured project schema with confidence levels and source evidence. Use when the user says "extract my project data", "analyze my docs", "pull the structured data", "pull out the fields", "extract from the chunks", "populate my schema", or after /ingest completes and the user is ready to structure their project data. Also auto-fires when /quick-start triggers extraction after ingestion. Do NOT use to find missing data or ask follow-up questions → use /gap-detect instead. Do NOT use to parse raw documents → use /ingest first. Do NOT use to generate the case study → use /generate instead.
---

# /extract — Field-Level Structured Extraction

## Step 0: Read Before Extracting

Read these files before producing any output:

| Source | Path | What to extract |
|--------|------|-----------------|
| Project schema | templates/project-schema.json | Field names and structure — the extraction target |
| Project inputs | context-library/project-inputs-[slug].md | Ingested document chunks — source material for all extraction |
| Extraction prompts | prompts/extraction-prompts.md | Per-field extraction guidance |

**If the slug is not known:** Ask "What's the project name? I'll use it to locate the input file." and wait. Do not proceed until the slug is confirmed.

**If `context-library/project-inputs-[slug].md` does not exist:** Stop. Say: "I can't extract without ingested content. Run /ingest first to process your documents, then run /extract." Do not proceed.

**If the file exists but contains no document chunks (only the schema template with empty fields):** Stop. Say: "The project inputs file exists but has no ingested content yet. Run /ingest to add document chunks, then re-run /extract."

## Step 1: Load Schema Target

Reference `templates/project-schema.json` as the extraction target. You are populating each field in this schema.

## Step 2: Run Per-Field Extraction

For each field in the schema, run a focused extraction pass across all document chunks.

**Critical rule: Extract only what is explicitly stated. Do not infer or fill gaps.**

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

## Step 3: High-Priority Fields

Address all 7 of these fields explicitly — even if confidence is `none`. Do not skip any.

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
Extract actual findings that changed something — not just methods used. "We ran 20 interviews" is a method. "Users were dropping because of X, not Y as we assumed" is an insight. For each insight, extract a short memorable label plus the explanation.

**pull_quotes**
Extract any verbatim user quotes, customer verbatims, field observations, or memorable PM statements. These ground the case study in a human moment. Only extract verbatim or near-verbatim language — do not paraphrase into pull quote form.

Also extract:

**solution.breakdown_principle**
Extract the structural principle that most naturally organizes the solution: by user type, by feature or UX decision, or by pipeline component. This determines how the Solution section is formatted.

## Step 4: Merge Across Chunks

When the same field appears in multiple chunks, merge the strongest evidence. Prioritize:
- Specificity over vagueness
- Hard numbers over approximations
- Explicit first-person ownership over team-level language

Surface conflicts explicitly: "Your PRD says X, but your resume bullet says Y — which is correct?"

## Step 5: Write to Context Library

Write extracted values to `context-library/project-inputs-[slug].md`. Fill in only fields with evidence — leave placeholders for fields with `none` confidence.

## Step 6: Extraction Summary (mandatory — produce this in your response before anything else)

Produce this exact block in your response. Do not skip it. Do not combine it with /gap-detect output. Do not produce it internally only.

```
Extraction complete.
Strong data on: [name specific fields — not just counts]
Needs clarification: [name specific fields with low or none confidence — will be handled in /gap-detect]
Conflicts found: [list any contradictions, or "none"]
```

This block must appear visibly in your response output before any /gap-detect content.

For each "Strong data on" field, cite the source chunk in parentheses. For each "Needs clarification" field, say what evidence would have confirmed it but wasn't found.

Example of a correctly produced Step 6 summary:

> Extraction complete.
> Strong data on: problem statement (PRD: "5-step process was the primary friction point"), primary outcome (Retro: "abandonment dropped 18% in the first 30 days"), named strategic decision (Retro: "considered keeping the 5-step flow… chose the collapse approach"), discovery insight (Interview: "users weren't confused by the steps — they were anxious about security"), solution breakdown by_feature (step collapse + trust badge as two discrete decisions).
> Needs clarification: role_scope.what_i_owned (PRD has "I led the redesign effort" — no sub-task ownership or PM title stated), pull_quotes (no verbatim user language found in any chunk — interview notes paraphrased only), company/product context (absent from all chunks).
> Conflicts found: none.
> Proceeding to gap detection...

## Step 7: Auto-Proceed

Self-check before proceeding: Does the Step 6 extraction summary appear above in your response as a distinct, visible block? If yes, continue to /gap-detect. If no, produce Step 6 first — then continue.

After the Step 6 summary is confirmed visible, automatically continue to /gap-detect unless the user asks to stop.

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "The conversation has enough context — I can skip Step 0" | Step 0 reads the project-inputs file and schema. Skipping it means extracting from training data, not source documents. |
| "I know what these fields should contain — evidence quotes aren't necessary" | Every extracted value must carry an evidence array with source quotes. Values without source quotes are fabrication, not extraction. |
| "pull_quotes and breakdown_principle are usually empty — I can skip them" | All 7 high-priority fields plus breakdown_principle must be addressed explicitly, even if marked `confidence: none`. Silent omission corrupts the schema. |
| "I did the extraction internally — I can just show the gap-detect questions directly" | Internal computation does not count. The Step 6 extraction summary must appear as visible text in your response, as a distinct block, before any /gap-detect output. If you can't point to it in your response, you haven't done Step 6. |
| "I'll combine the Step 6 summary with the /gap-detect output" | The extraction summary must appear as a distinct, visible block before /gap-detect output. The self-check in Step 7 enforces this. |
| "I can auto-proceed to /gap-detect without showing Step 6" | Step 7 has a self-check: confirm Step 6 appears in your response before proceeding. If it doesn't, produce Step 6 first. |

## Before Marking Complete

Do not consider this task finished until all of the following are true:

- [ ] Step 0: Named which files were read (project-inputs-[slug].md, project-schema.json, extraction-prompts.md)
- [ ] All 7 high-priority fields from Step 3 explicitly addressed — each has a confidence level and at least one evidence quote, OR is explicitly marked `confidence: none`
- [ ] solution.breakdown_principle addressed
- [ ] No invented values — every non-"none" field is traceable to a source quote from the project-inputs file
- [ ] Conflicting signals across chunks surfaced explicitly
- [ ] Step 6 summary shown to user before auto-proceeding to /gap-detect (names specific fields, not just counts)
- [ ] Wrote to context-library/project-inputs-[slug].md

## Out of Scope

This skill does NOT handle:
- Finding what data is missing or asking follow-up questions → use /gap-detect instead
- Parsing or chunking raw documents → use /ingest first, then /extract
- Validating evidence strength or narrative angle → use /evidence-check instead
- Writing or generating the case study → use /generate instead
- Starting the full pipeline from scratch → use /quick-start instead

## Worked Example (Phase 1 → Phase 2)

**Phase 1 — user runs `/extract` with no prior context:**

> What's the project name? I'll use it to locate the input file.

User responds: "checkout-redesign"

**Phase 2 — slug provided, extraction runs:**

Step 0: Reading `context-library/project-inputs-checkout-redesign.md`, `templates/project-schema.json`, `prompts/extraction-prompts.md`.

[All steps 1-5 run — per-field extraction, high-priority fields, merge, write to file]

```
Extraction complete.
Strong data on: problem statement (PRD: "5-step process was the primary friction point"), primary outcome (Retro: "abandonment dropped 18% in the first 30 days"), named strategic decision (Retro: "considered keeping the 5-step flow… chose the collapse approach"), discovery insight (Interview: "users weren't confused by the steps — they were anxious about security"), solution breakdown by_feature (step collapse + trust badge as two discrete decisions).
Needs clarification: role_scope.what_i_owned (PRD has "I led the redesign effort" — no sub-task ownership or PM title stated), pull_quotes (no verbatim user language found — interview notes paraphrased only), company/product context (absent from all chunks).
Conflicts found: none.
```

Proceeding to /gap-detect...

## Rules

- Extract only what is explicitly in the source. Mark gaps honestly.
- Preserve exact numbers, percentages, dates, and quotes from source documents.
- If the same data point appears with different values across documents, surface the conflict.
- Do not output the full schema to the user — give the Step 6 summary, not a data dump.
