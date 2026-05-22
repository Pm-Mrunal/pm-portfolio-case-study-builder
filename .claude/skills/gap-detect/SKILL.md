---
name: gap-detect
description: Identify missing or weak fields in the extracted PM case study schema and generate 3-5 targeted questions to fill the highest-impact gaps before generation. Use when the user says "what am I missing", "what do you still need", "do you need more from me", "I'm not sure I have enough", "what's missing from my case study", "what gaps do I have", "check what's missing", or after /extract completes. Auto-triggered by /quick-start after extraction and before /generate when the user manually fills a project-inputs file. Do NOT use for validating evidence strength or finding the strongest narrative angle → use /evidence-check instead. Do NOT use to write or generate the case study → use /generate instead. Do NOT use to start a new project from scratch → use /quick-start instead.
---

# /gap-detect — Gap Detection and Targeted Questions

## Step 0: Load Context

Read `references/learnings.md` if it exists. Apply patterns flagged as successful. Avoid patterns flagged as failures.

| Source | Path | What to extract |
|--------|------|-----------------|
| Project schema | `context-library/project-inputs-[slug].md` | All field values, confidence levels, blank fields |
| Writing preferences | `context-library/writing-preferences.md` | Tone, target role, emphasis — shapes which gaps matter most |

If the slug is not in context, ask: "Which project are we working on?" and list any existing `project-inputs-*.md` files in `context-library/`. Do not proceed until the slug is confirmed.

## Step 1: Load Schema

Read the current state of `context-library/project-inputs-[slug].md` for the active project. Identify all fields with `low` or `none` confidence, or blank values.

## Step 2: Score Gap Severity

Not all gaps matter equally. Score each gap by its impact on case study quality:

**Critical gaps (always ask if missing):**
- Role ownership — what the user personally owned vs. contributed to vs. supported
- Problem statement — clear articulation of who had the problem, the prior state, and business stakes. Must be specific enough to support 3-4 named problem dimensions.
- Outcomes — any quantitative result, directional signal, or validated observation
- Key decisions with alternatives — at least one decision where alternatives were explicitly considered and rejected

**High-value gaps (ask if 1-2 slots remain after critical):**
- Named decisions (minimum 2) — distinct, nameable product or strategic calls with rationale. If 4+ exist, a decision table becomes viable.
- Pull quote material — any verbatim user quote, customer verbatim, field observation, or memorable PM statement from the source. Grounds the case study in a human moment.
- Named discovery insights — insights labeled with a specific memorable finding, not just described in prose
- Solution breakdown — which structural principle organizes the solution (by user type, by feature, by pipeline component). If unclear, the Solution section defaults to undifferentiated prose.

**Lower-priority gaps (skip unless nothing else is missing):**
- Specific "what I'd do differently" — one decision, its consequence, what they'd have done instead
- Team composition specifics
- Exact timeline dates
- Secondary outcomes or execution milestones

## Step 3: Select Questions

Select the 3-5 highest-severity gaps. Write one clear, specific question per gap.

**Question writing rules:**
- Ask for specifics, not summaries. "What metric moved, and by how much?" not "What were the results?"
- Make the question easy to answer even if the user doesn't have data. "What's the strongest outcome you can point to — even a directional signal counts."
- Give the user permission to be imprecise. "Even an approximate number or a qualitative observation helps."
- Never ask more than 5 questions. If you have 6 gaps, pick the 5 that matter most.

## Step 4: Present Questions

Display questions using this exact format:

"Before I generate your case study, I need a few details to make it as strong as possible:

1. [Question about highest-priority gap]
2. [Question about second gap]
3. [Question about third gap]
[4. Optional fourth]
[5. Optional fifth]

Answer as much as you can — rough estimates and directional observations count."

**Worked example (search-ranking project, 4 gaps identified):**

> Before I generate your case study, I need a few details to make it as strong as possible:
>
> 1. What is the strongest outcome you can point to for the search ranking work? Even an approximate number helps — a change in a search quality metric, click-through rate, or time-to-result. A directional signal with context ("query success rate improved meaningfully in our holdout test") is stronger than a general statement.
>
> 2. Do you have any user or stakeholder quotes from this project — something a researcher, customer, or teammate said that captured what was wrong with search or what changed after? A single verbatim line, even a paraphrase you remember clearly, anchors the case study in a real human moment.
>
> 3. The discovery insight you described — can you give it a name or a sharp one-line summary? For example: "Users were querying by symptom, not product name." A labeled insight is far more memorable than prose.
>
> 4. For the key decisions you documented: were there alternatives you seriously considered and rejected? Even one example — "we debated X but chose Y because Z" — shows PM judgment.
>
> Answer as much as you can — rough estimates and directional observations count.

## Step 5: Process Answers

When the user responds:
- Extract the new information and merge it into the schema
- Tag newly added fields as `user_provided` with high confidence
- If an answer is still vague, accept it and proceed — do not ask follow-ups

## Step 6: Auto-Proceed

After answers are received, automatically proceed to `/generate` unless the user asks to stop.

## Edge Cases

**If the user's documents are very strong (all critical fields have high confidence):**
Say: "Your documents are detailed — I have strong data on all the key fields. Running generation now." Skip questions unless there is at least one critical gap.

**If the user skips a question or says "I don't know":**
Accept it and proceed. Generate using safe phrasing for that field. Do not re-ask.

**If the user's answers raise a conflict with the extracted data:**
Flag it before generating: "You mentioned X in your notes, but you're saying Y now — which should I use?"

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|----------------------|----------------|
| "The user provided project context in chat — I can skip reading the file" | The file is the ground truth. Chat summaries miss field-level confidence values and may be stale. |
| "All critical gaps are mentioned — I can skip scoring severity" | Severity scoring determines which 3-5 questions matter. Skipping it produces questions ranked by schema order, not impact. |
| "I've seen this project in the conversation — I can guess the gaps" | Read the current file state. Fields change between sessions. |
| "The user said 'generate' — I'll skip to /generate immediately" | Run gap detection first. Only auto-proceed to /generate after answers are received per Step 6. For pure generate requests, route to /generate instead. |
| "5 gaps exist — I should ask 5 questions" | Select the 3-5 HIGHEST-severity gaps, not all gaps. If all critical fields are strong, state that and skip questions. |

## Before Marking Complete

Do not consider this task finished until all of the following are true:

- [ ] Project slug confirmed and `context-library/project-inputs-[slug].md` read
- [ ] All fields scanned for low/none confidence or blank values
- [ ] Gap severity scored — critical vs. high-value vs. lower-priority
- [ ] 3-5 questions selected by severity (not by schema order)
- [ ] Questions presented with opener and "Answer as much as you can" footer
- [ ] If strong documents (all critical fields high confidence): stated that explicitly and skipped questions
- [ ] After user answers: new values merged into schema with `user_provided` tag
- [ ] Auto-proceeded to `/generate` (or stopped if user asked to)

## Out of Scope

This skill does NOT handle:
- Validating evidence strength or finding the strongest narrative angle → use `/evidence-check`
- Writing or generating the case study → use `/generate`
- Starting a new project from raw documents → use `/quick-start`
- Ingesting or parsing raw project documents → use `/ingest`
- Identifying which case study angle is strongest for senior roles → use `/evidence-check`

## After Completing: Log Learning

Append one entry to `references/learnings.md` (create if it doesn't exist):

```
Date: [today]
Project slug: [slug]
Questions asked: [N]
What worked: [specific pattern that produced a useful response from the user]
What didn't: [any question that got a "I don't know" or blank]
Edge case: [anything unexpected — strong documents, conflict, ambiguous ownership]
```
