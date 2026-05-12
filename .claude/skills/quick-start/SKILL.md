---
name: quick-start
description: Runs the full case study pipeline in a single command from raw pasted documents. Use when the user says "quick start", "build my case study", "make a case study", "help me write a case study", "turn my notes into a case study", "write a case study from scratch", "I'm starting fresh", "fast draft", or pastes project documents and asks for a portfolio piece without setup. Auto-fires when a user has project materials and wants to produce a case study without running individual pipeline steps. Do NOT use for an existing project-slug that already has a case study → use /generate instead. Do NOT use to build a portfolio website → use /build-portfolio instead.
---

# /quick-start — Full Pipeline in One Command

## Step 0: Read Before Starting

Read these before producing any output:

| Source | Path | What to extract |
|--------|------|-----------------|
| Writing preferences | `context-library/writing-preferences.md` | Tone, targeting, section emphasis — apply if filled; skip silently if blank |
| Existing output slugs | `outputs/` directory listing | Names of existing case study files — warn user if this slug already exists |
| Extraction schema | `prompts/extraction-prompts.md` | Field list and confidence-tracking format to use in Step 3 |
| Generation prompt | `prompts/generation-prompt.md` | TLDR structure, detailed version format, and output template to follow in Step 5 |
| Polish prompt | `prompts/polish-prompt.md` | Quality rewrite rules to apply in Step 6 |
| Visual planning prompt | `prompts/visual-planning-prompt.md` | Three-prompt sequence to run in Step 6b |

Do not produce any case study output until Step 0 is complete.

If no documents were pasted, stop here. Say: "Paste your project documents to begin — any combination of PRD, notes, metrics, research, or resume bullets works. Once you paste something, I'll run the full pipeline."

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

### Step 6: Polish

Run `prompts/polish-prompt.md`. No new facts added. Improve flow, clarity, and PM signal only.

This step is not optional. Do not skip it because the output reads well.

### Step 6b: Visual Layer

After polish, run `prompts/visual-planning-prompt.md`. This does not change any factual content.

Run its three prompts in sequence:
1. VISUAL PLANNING PROMPT → `visual_plan` (4-8 visuals)
2. INSERT PLACEHOLDERS PROMPT → `[VISUAL: ...]` markers inserted into both texts
3. GENERATE CAPTIONS PROMPT → `visual_captions`

This step always runs. It requires no user input.

## Before Marking Complete

Do not proceed to Step 7 (save) until all items are confirmed:

- [ ] Step 0 complete — all prompt files confirmed present
- [ ] Documents were pasted — if not, stopped and asked before Step 1
- [ ] Step 2 parse/classify ran on ALL pasted content
- [ ] Step 3 schema extraction ran using extraction-prompts.md
- [ ] Step 4 gap questions asked (3-5 max) AND answers received before generating
- [ ] Step 5 generation ran using generation-prompt.md — TLDR (5 sub-blocks) + detailed version (10 sections) both produced
- [ ] Step 6 polish ran using polish-prompt.md — no new facts added
- [ ] Step 6b visual layer ran — visual plan JSON, [VISUAL: ...] placeholders in both texts, and captions JSON all present
- [ ] No em dashes in generated text
- [ ] No bold markdown in case study body text
- [ ] No invented metrics, percentages, or outcomes
- [ ] Role ownership language matches what was explicitly stated in source documents
- [ ] Launch status stated correctly — use "pilot", "pre-launch", or "MVP" if unclear
- [ ] Output file named correctly: `outputs/[slug]-v1-[YYYY-MM-DD].md`

### Step 7: Save

Save to `outputs/[project-slug]-v1-[YYYY-MM-DD].md`. The file includes both case study versions with visual placeholders, the visual plan JSON, and the captions JSON.

Tell the user: "Your case study is saved to `outputs/[filename]`. Visual plan and captions are included at the end. Run `/polish` for another language pass or `/evidence-check` to validate your evidence before sharing."

## Output Format

```
=== RECRUITER TLDR ===
[At a glance | The Problem | Why This Matters | What I Owned | Key Results]
[350-500 words, with [VISUAL: ...] placeholders]

=== DETAILED HIRING MANAGER VERSION ===
[Ten sections with editorial headlines]
[1,800-2,800 words, with [VISUAL: ...] placeholders]

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

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|----------------------|----------------|
| "Skip Step 4 gap questions because the documents are detailed enough" | Even detailed documents miss ownership clarity and key decisions. Ask the 3 highest-impact questions. Never skip Step 4. |
| "Skip Step 6 polish because the output reads well" | Polish is required every run. Output that reads well still benefits from PM signal tightening. Run the polish prompt. |
| "Skip Step 6b visual layer because no visuals were mentioned" | Visual layer always runs — it adds placeholders even when the user didn't mention visuals. No user input required. |
| "Run quality checks after saving" | Quality checks must run BEFORE Step 7 (save). A check after save is advisory, not blocking. |
| "Ask 5-7 questions to be thorough" | Maximum 5 questions. If confident with 3, ask only 3. Never ask 6 or 7. |
| "Route to /review-as-recruiter when done" | That skill does not exist. Route to /polish or /evidence-check instead. |
| "I can proceed without reading prompts/generation-prompt.md" | The output format (TLDR sub-blocks, detailed section count) is defined in that file. Skipping Step 0 produces a format that drifts from session to session. |

## Cross-Skill Routing

- If the user already has an existing case study file and wants to regenerate → suggest `/generate [project-slug]` instead
- If the user wants to refine an existing case study → suggest `/polish`
- If the user wants to validate their evidence before generating → suggest `/evidence-check`
- If the user wants to publish their case studies as a website → suggest `/build-portfolio`
