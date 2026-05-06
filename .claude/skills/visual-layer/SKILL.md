---
name: visual-layer
description: Post-processing layer that runs after case study generation. Produces a visual plan, inserts placeholders into both outputs, and generates captions. Does not modify the case study's factual content or generation logic.
---

# /visual-layer — Visual Planning, Placeholders, and Captions

## When to Use

- Automatically after `/generate` completes (called internally by the generate pipeline)
- Manually when the user says "add visuals" or "add visual placeholders"
- When re-running visuals on an existing output: `/visual-layer [paste or reference output file]`

## What This Skill Does NOT Do

- Does not modify the factual content of the case study
- Does not change tone, structure, or generation logic
- Does not generate actual images
- Does not reference tools like Figma unless the user explicitly mentioned them
- Does not claim visuals exist unless they were mentioned in source inputs
- Does not invent visual content that isn't grounded in the project data

## Inputs

- `tldr_case_study` — the generated TLDR text (from `/generate`)
- `detailed_case_study` — the generated detailed version text (from `/generate`)
- `project_schema` — the structured project data from `context-library/project-inputs.md`
- `prompts/visual-planning-prompt.md` — all three step prompts

## Pipeline

This skill runs three steps in sequence:

```
Step 1: Visual Planning      →  visual_plan JSON
Step 2: Insert Placeholders  →  modified tldr + modified detailed
Step 3: Generate Captions    →  visual_captions JSON
```

All three outputs are assembled into the final envelope before saving.

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

## Quality Checks Before Finishing

- [ ] Between 4 and 8 visuals in the plan
- [ ] No placeholder breaks sentence flow
- [ ] TLDR has no more than 3 placeholders
- [ ] Detailed version has no more than 6 placeholders
- [ ] Every placeholder matches an entry in `visual_plan` (by `visual_type`)
- [ ] Every caption is 1-2 sentences, present tense
- [ ] No factual content changed in either case study
- [ ] No tools (Figma, Miro, etc.) referenced unless user mentioned them
- [ ] No images claimed to exist unless user confirmed them
