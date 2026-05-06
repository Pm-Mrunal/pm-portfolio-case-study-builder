---
name: gap-detect
description: Identify missing or weak fields in the extracted schema. Generate 3-5 targeted questions to fill the highest-impact gaps before generation.
---

# /gap-detect — Gap Detection and Targeted Questions

## When to Use

- After `/extract` has built the schema
- User says "what am I missing?" or "what do you still need?"
- Auto-triggered by `/quick-start` after extraction
- Before running `/generate` when the user has manually filled `context-library/project-inputs-[slug].md`

## Inputs

- Populated schema from `/extract` or `context-library/project-inputs-[slug].md`
- `prompts/gap-detection-prompt.md`

## Process

### Step 1: Load Schema

Read the current state of `context-library/project-inputs-[slug].md` for the active project. If the slug is not in context, ask: "Which project are we working on?" and list any existing `project-inputs-*.md` files in `context-library/`. Identify all fields with `low` or `none` confidence, or blank values.

### Step 2: Score Gap Severity

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

### Step 3: Select Questions

Select the 3-5 highest-severity gaps. Write one clear, specific question per gap.

**Question writing rules:**
- Ask for specifics, not summaries. "What metric moved, and by how much?" not "What were the results?"
- Make the question easy to answer even if the user doesn't have data. "What's the strongest outcome you can point to — even a directional signal counts."
- Give the user permission to be imprecise. "Even an approximate number or a qualitative observation helps."
- Never ask more than 5 questions. If you have 6 gaps, pick the 5 that matter most.

### Step 4: Present Questions

Display questions clearly, numbered:

"Before I generate your case study, I need a few details to make it as strong as possible:

1. [Question about highest-priority gap]
2. [Question about second gap]
3. [Question about third gap]
[4. Optional fourth]
[5. Optional fifth]

Answer as much as you can — rough estimates and directional observations count."

### Step 5: Process Answers

When the user responds:
- Extract the new information and merge it into the schema
- Tag newly added fields as `user_provided` with high confidence
- If an answer is still vague, accept it and proceed — do not ask follow-ups

### Step 6: Auto-Proceed

After answers are received, automatically proceed to `/generate` unless the user asks to stop.

## Edge Cases

**If the user's documents are very strong (all critical fields have high confidence):**
Say: "Your documents are detailed — I have strong data on all the key fields. Running generation now." Skip questions unless there is at least one critical gap.

**If the user skips a question or says "I don't know":**
Accept it and proceed. Generate using safe phrasing for that field. Do not re-ask.

**If the user's answers raise a conflict with the extracted data:**
Flag it before generating: "You mentioned X in your notes, but you're saying Y now — which should I use?"
