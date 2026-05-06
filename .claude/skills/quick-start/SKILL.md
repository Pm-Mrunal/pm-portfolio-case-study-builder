---
name: quick-start
description: Full pipeline in one command. Paste project documents and get a recruiter TLDR + detailed case study with no prior setup. Best for first-time users and fast drafts.
---

# /quick-start — Full Pipeline in One Command

## When to Use

- User says `/quick-start` and pastes documents
- User says "build my case study" with documents attached
- First interaction — give immediate value before asking for full setup
- User has messy notes and wants a draft fast

## What This Skill Does

Runs the complete pipeline in a single pass:
1. Parses and classifies pasted documents
2. Extracts structured schema field-by-field
3. Identifies gaps, asks 5-7 targeted questions
4. Generates recruiter TLDR + detailed case study
5. Runs quality polish pass
6. Saves output to `outputs/`

## Inputs

- Raw pasted text — any combination of PRD, notes, research, metrics, resume bullets
- Or: file paths to documents in the project folder
- `context-library/writing-preferences.md` — auto-loaded if filled

## Important: Always Starts Fresh

`/quick-start` always creates a brand new project input file. It never reads from or overwrites an existing `project-inputs-*.md`. If a file for the same project slug already exists, ask the user whether to use a different name or overwrite before proceeding.

The new file is named `context-library/project-inputs-[slug].md` where `[slug]` is derived from the project name found in the pasted documents.

## Process

### Step 1: Acknowledge

Tell the user:
"I'll extract your project data, identify what's missing, ask a few targeted questions, and generate your case study. This takes 3-5 minutes."

### Step 2: Parse and Classify

Parse all pasted content. Classify each section as one of:
- `prd` — product brief or requirements doc
- `research` — user research, interviews, synthesis
- `metrics` — data, analytics, outcomes
- `strategy` — strategy docs, decision memos, roadmap
- `resume` — resume bullets or LinkedIn
- `notes` — raw notes, Slack threads, personal observations

### Step 3: Extract Schema

Run field-level extraction from `prompts/extraction-prompts.md` against all content.

For each field, track:
- `value` — what was extracted
- `confidence` — high / medium / low / none
- `evidence` — supporting quote or paraphrase
- `needs_user_input` — true if confidence is low/none for a high-priority field

### Step 4: Gap Questions

Identify the 3-5 highest-impact missing fields. Priority order:
1. Role ownership clarity — what did you personally own vs. contribute to vs. support?
2. Problem statement — who, prior state, business stakes, and 3-4 distinct nameable dimensions
3. Key decisions with alternatives — at least one decision where alternatives were considered and rejected
4. Outcomes — any quantitative results or validation signals
5. Named decisions (minimum 2) and pull quote material — verbatim user quotes, field observations, or memorable statements
6. Solution breakdown — what structural principle organizes the solution (by user, by feature, or by component)

Ask questions one at a time or as a numbered list. Wait for answers before generating.

**Maximum 5 questions. If you can generate confidently with 3, ask only 3.**

If the user's documents are thin (under 200 words total), say: "Your documents are brief. The more context you can share, the stronger the output. Add anything — rough notes, bullet points, even a verbal brain dump works."

### Step 5: Generate

Load `context-library/writing-preferences.md` if filled. Merge with extracted schema and answers.

Run `prompts/generation-prompt.md`.

Produce:
- Recruiter TLDR (350-500 words, five labeled sub-blocks: At a glance / The Problem / Why This Matters / What I Owned / Key Results)
- Detailed hiring manager version (1,800-2,800 words, ten sections with editorial headlines)
- 3 portfolio headline options
- 2 subtitle options
- Up to 5 visual caption suggestions (if user mentioned visuals)

### Step 6: Polish

Run `prompts/polish-prompt.md`. No new facts added. Improve flow, clarity, and PM signal only.

### Step 6b: Visual Layer

After polish, run `/visual-layer` as a post-processing step. This does not change any factual content.

Load `prompts/visual-planning-prompt.md` and run its three prompts in sequence:
1. VISUAL PLANNING PROMPT → `visual_plan` (4-8 visuals)
2. INSERT PLACEHOLDERS PROMPT → `[VISUAL: ...]` markers inserted into both texts
3. GENERATE CAPTIONS PROMPT → `visual_captions`

This step always runs. It requires no user input.

### Step 7: Save

Save to `outputs/[project-slug]-v1-[YYYY-MM-DD].md`. The file includes both case study versions with visual placeholders, the visual plan JSON, and the captions JSON.

Tell the user: "Your case study is saved to `outputs/[filename]`. Visual plan and captions are included at the end. Run `/review-as-recruiter` or `/review-as-hiring-manager` for a detailed critique."

## Output Format

```
=== RECRUITER TLDR ===
[450-650 words, with [VISUAL: ...] placeholders]

=== DETAILED HIRING MANAGER VERSION ===
[1,200-2,000 words, with [VISUAL: ...] placeholders]

=== HEADLINES + SUBTITLES ===
Headline 1: ...
Headline 2: ...
Headline 3: ...
Subtitle 1: ...
Subtitle 2: ...

=== VISUAL PLAN ===
[visual_plan JSON]

=== VISUAL CAPTIONS ===
[visual_captions JSON]
```

## Quality Checks Before Finishing

- No invented metrics, percentages, or outcomes
- Role ownership language matches what was explicitly stated
- If launch status is unclear, output says "pilot" or "pre-launch" — not "launched"
- No em dashes in generated text
- No bold markdown in case study body
- Output saved to `outputs/` before closing
