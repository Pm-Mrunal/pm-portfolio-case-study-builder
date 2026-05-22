---
name: visual-layer
description: Adds a visual plan, [VISUAL: ...] placeholder tags, and captions to a generated PM portfolio case study. Use when the user says "add visuals", "add visual placeholders", "add the visual layer", "make it more visual", "what charts should I add", "add diagrams to my case study", "insert visual placeholders", or after /generate completes. Auto-fires at the end of the /generate pipeline. Does not modify factual content — only adds visual annotations. Do NOT use to rewrite or edit content → use /polish. Do NOT use to add new facts → use /generate or /gap-detect. Do NOT use to build the portfolio site → use /build-portfolio.
---

# /visual-layer — Visual Planning, Placeholders, and Captions

## What This Skill Does NOT Do

- Does not modify factual content, tone, structure, or generation logic → if you need content edits, use /polish
- Does not generate actual images → create visuals in a design tool after the plan is complete
- Does not reference tools like Figma unless the user explicitly mentioned them
- Does not claim visuals exist unless they were mentioned in source inputs
- Does not invent visual content that isn't grounded in the project data

## Step 0: Detect Inputs Before Running

Check inputs in this order before executing any pipeline steps:

| Source | Path | What to extract |
|--------|------|-----------------|
| Case study output | `outputs/[project-slug]-v*.md` (user-supplied or most recent for slug) | TLDR text, detailed version text, whether `## VISUAL PLAN` section already exists |
| Project schema | `context-library/project-inputs-[project-slug].md` | Structured project data for visual planning |
| Visual prompts | `prompts/visual-planning-prompt.md` | Prompt templates for Steps 1–3 |

**If no case study text was provided and no output file was referenced:**
Stop. Do not proceed. Say: "Which case study should I add the visual layer to?" List all files in `outputs/` that do not already contain a `## VISUAL PLAN` section.

**If the output file already contains a `## VISUAL PLAN` section:**
Do not re-run the full pipeline. Surface the existing visual plan to the user as a summary table. Say: "The visual layer is already applied to this file. Here's the current plan: [summary table]. Say 're-run visual layer' if you want a fresh pass."

**If the output file exists and has no `## VISUAL PLAN` section:**
Proceed to Step 1.

Only proceed past Step 0 once inputs are confirmed.

## Pipeline

This skill runs four steps in sequence:

```
Step 0: Detect Inputs        →  confirm case study + schema available
Step 1: Visual Planning      →  visual_plan JSON
Step 2: Insert Placeholders  →  modified TLDR + modified detailed version
Step 3: Generate Captions    →  visual_captions JSON
Step 4: Assemble + Save      →  final file written to outputs/
```

All outputs are assembled into the final file before saving.

## Common Shortcuts — Do Not Take These

| What Claude might think | Why it's wrong |
|---|---|
| "The user mentioned the project name — I can infer the case study content" | Step 0 requires reading the actual output file. Inferred content produces invented visual placements and captions that aren't grounded in the real text. |
| "The visual layer was already applied — I'll quietly add a couple more visuals" | If the layer is already applied, surface the existing plan and wait for confirmation. Do not silently modify the file. |
| "I can skip Step 3 (captions) — the visual plan is enough" | Captions are required in the final output envelope. The portfolio assets section is incomplete without them. |
| "The TLDR is short, I'll add more than 3 placeholders to be helpful" | Maximum 3 placeholders in the TLDR is a hard rule — it prevents visual overload in the short version. |
| "Step 4 assembly is obvious — I'll describe the structure instead of producing it" | Step 4 must produce and save the actual assembled file. Describing it is not the deliverable. |

---

## Step 1: Visual Planning

Run `prompts/visual-planning-prompt.md` → VISUAL PLANNING PROMPT against the project schema.

**Input:** structured project data
**Output:** `visual_plan` JSON object

Rules:
- Recommend between 4 and 8 visuals — no fewer, no more
- Cover high-impact sections only: problem, users, solution, workflow, execution, outcomes
- Every visual must have a specific `description` — not just a type label
- Set `exists_in_inputs: true` only if the user explicitly mentioned a screenshot, diagram, or image in their source documents; otherwise set `false` and mark `creation_needed: true`
- If no relevant visuals were mentioned, recommend what should be created — do not skip the plan

```json
{
  "visuals": [
    {
      "id": "v1",
      "section": "Problem",
      "placement_hint": "after problem paragraph",
      "visual_type": "Before-state workflow diagram",
      "description": "Step-by-step user journey showing the 7-step onboarding flow and the specific step where 71% of users dropped off",
      "why_it_matters": "Makes the abstract drop-off statistic concrete and shows what the PM was diagnosing",
      "exists_in_inputs": false,
      "creation_needed": true
    }
  ]
}
```

**Section coverage guidance:**

| Section | Recommended visual types |
|---|---|
| Problem | Before-state workflow diagram, friction map, drop-off funnel screenshot |
| Users | User persona card, segmentation diagram, research insight summary |
| Strategy / decisions | Options comparison table, decision matrix, tradeoff diagram |
| Solution | After-state workflow diagram, product screenshot, annotated UI |
| Execution | Timeline or milestone chart, iteration comparison (v1 vs v2) |
| Outcomes | Metrics chart, before/after comparison, funnel improvement visualization |

---

## Step 2: Insert Visual Placeholders

Run `prompts/visual-planning-prompt.md` → INSERT PLACEHOLDERS PROMPT.

**Input:** `tldr_case_study` text, `detailed_case_study` text, `visual_plan`
**Output:** both texts with placeholders injected

Placeholder format — use exactly this syntax:
```
[VISUAL: <visual_type> – <short description>]
```

Rules:
- Insert placeholders at logical breakpoints: end of a section, after a key paragraph, after a statistic is introduced
- Do not interrupt sentence flow — always place at the end of a paragraph or between paragraphs
- Do not insert more than 3 placeholders in the TLDR (it's short — keep it clean)
- Do not insert more than 6 placeholders in the detailed version
- Each placeholder must correspond to an entry in `visual_plan`
- Do not repeat the same placeholder type in adjacent sections
- Prioritise the detailed version — the TLDR gets a subset of the most impactful placements

**TLDR placement strategy:**
Place 2-3 placeholders at the highest-impact moments: typically after the problem statement, after the solution description, and in or after the results section.

**Detailed version placement strategy:**
Distribute across sections. Each major section (Problem, Solution, Outcomes) should have at most one placeholder. Do not cluster placeholders together.

**Correct example:**
```
Problem

The onboarding flow required new users to complete 9 steps before seeing any product value. Session recordings showed 71% of signups dropped at step 3 — the data source connection — because it required an API key they didn't have on hand.

[VISUAL: Before-state workflow diagram – 9-step onboarding flow with step-3 drop-off highlighted]

This wasn't a complexity problem. It was a setup friction problem.
```

**Incorrect example (do not do this):**
```
The onboarding [VISUAL: diagram] flow required users to complete...
```

---

## Step 3: Generate Visual Captions

Run `prompts/visual-planning-prompt.md` → GENERATE CAPTIONS PROMPT.

**Input:** `visual_plan`
**Output:** `visual_captions` JSON

Rules:
- One caption per visual in the plan
- Each caption must be 1-2 sentences maximum
- Explain what the visual shows — do not editorialize or add claims
- Where possible, tie the caption to a product insight or decision it supports
- Write in present tense ("The diagram shows..." not "The diagram showed...")
- Do not invent visual content — describe what the visual would show based on project data

```json
{
  "captions": [
    {
      "id": "v1",
      "visual_type": "Before-state workflow diagram",
      "caption": "The original 9-step onboarding flow, showing the data source connection at step 3 where session recordings identified the primary drop-off point."
    }
  ]
}
```

---

## Step 4: Assemble Final Output

Combine all three steps into the final output envelope:

```json
{
  "tldr_case_study": "...full TLDR text with [VISUAL: ...] placeholders...",
  "detailed_case_study": "...full detailed text with [VISUAL: ...] placeholders...",
  "visual_plan": {
    "visuals": [...]
  },
  "visual_captions": {
    "captions": [...]
  }
}
```

When saving to file, write:
```
# [Project Title] — Case Study

---

## RECRUITER TLDR

[TLDR with placeholders]

---

## DETAILED HIRING MANAGER VERSION

[Detailed version with placeholders]

---

## PORTFOLIO ASSETS

Headlines: ...
Subtitles: ...

---

## VISUAL PLAN

[visual_plan JSON, pretty-printed]

---

## VISUAL CAPTIONS

[visual_captions JSON, pretty-printed]
```

---

## Image Detection (Optional)

If the user mentions uploading screenshots, diagrams, or images during the session:

1. Note which visuals were described or referenced
2. In `visual_plan`, set `exists_in_inputs: true` for any placeholder that could be matched to an uploaded visual
3. Update the placeholder text to reflect that the visual exists: `[VISUAL: Before-state workflow diagram – upload provided]`
4. Do not attempt to parse or describe the image contents beyond what the user described

If no images were uploaded or mentioned, skip this step entirely.

---

## Before Marking Complete

Do not consider this task finished until all of the following are true:

- [ ] Step 0 run — confirmed case study file location and whether visual layer was already applied
- [ ] Between 4 and 8 visuals in visual_plan JSON
- [ ] No placeholder breaks sentence flow — all [VISUAL: ...] tags appear between paragraphs
- [ ] TLDR has no more than 3 placeholders
- [ ] Detailed version has no more than 6 placeholders
- [ ] Every placeholder matches an entry in visual_plan by visual_type
- [ ] Every caption is 1-2 sentences, present tense
- [ ] No factual content changed in either case study
- [ ] No tools (Figma, Miro, etc.) referenced unless user mentioned them
- [ ] No images claimed to exist unless user confirmed them
- [ ] Final file saved to outputs/ with all 5 sections present

If any item is unchecked, complete it before finishing.
