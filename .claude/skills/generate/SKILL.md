---
name: generate
description: Generates the recruiter TLDR and detailed hiring manager case study from the project schema. Use when the user says "generate my case study", "write my case study", "create my case study", "write it up", "write it now", "I'm ready to generate", "let's generate", "turn this into a case study", or names a project slug with intent to produce output. Auto-fires after /gap-detect when the user signals readiness. Do NOT use for building a portfolio site → use /build-portfolio. Do NOT use to identify missing data or ask follow-up questions → use /gap-detect instead.
---

# /generate — Case Study Generation

## Step 0: Read Before You Write

| Source | Path | What to extract |
|--------|------|-----------------|
| Project schema | `context-library/project-inputs-[slug].md` | All sections: project_basics, problem, role_scope, discovery, strategy, solution, outcomes, reflection, pull_quotes |
| Writing preferences | `context-library/writing-preferences.md` | Tone, seniority target, emphasis, any hard constraints |
| Generation prompt | `prompts/generation-prompt.md` | Core generation instructions and framing |
| TLDR template | `templates/recruiter-tldr.md` | Exact TLDR structure and sub-block sequence |
| Detailed template | `templates/hiring-manager-detailed.md` | Exact 10-section structure with editorial headline rules |

If `writing-preferences.md` is blank or missing, proceed without it — do not pause to ask.

## Pre-Generation Check

Before generating, confirm which project input file to use. If no slug is in context, list all `context-library/project-inputs-*.md` files and ask: "Which project should I generate? [list slugs]"

Then verify the selected file has:
- `project_basics.project_title` is present
- `problem.problem_statement` is present
- `role_scope.what_i_owned` has at least a partial entry
- `outcomes` has at least one entry (quantitative, qualitative, or validation signal)

If any of these are missing, do NOT generate. Say: "I'm missing [field name] before I can generate. Can you provide [brief description of what's needed]?"

## Process

### Step 1: Load Everything

Load the schema, writing preferences, and both templates.

Identify the output strategy based on available evidence:
- Strong outcomes available → lead with results
- No metrics but clear validation signals → lead with learning and decision quality
- Pre-launch or prototype → lead with problem quality and strategic thinking
- 0-to-1 project → lead with builder narrative and ambiguity navigation

### Step 2: Run Evidence Check (Internal)

Before writing, internally assess:
- What are the 2-3 strongest proof points available?
- What claims might be unsupported?
- What's the best narrative angle given the evidence?
- Are there any risk flags (vague ownership, no outcomes, weak causality)?

Adjust generation strategy accordingly. Do not surface this check to the user.

### Step 3: Generate Recruiter TLDR

Target: 350-500 words of content (not counting headers). Optimized for visual structure and fast scanning.

Structure (from `templates/recruiter-tldr.md`) — five labeled sub-blocks in this exact sequence:
1. Title block — project name, company, role, timeline, stage
2. At a glance — one sentence: company context + project summary
3. The Problem — 2-3 sentences: who, prior state, business stakes
4. Why This Matters — 1-2 sentences: structural or strategic reason this was worth solving
5. What I Owned — numbered list of 5-7 PM action phrases (not bullet points)
6. Key Results — 4-6 bullets, quantitative first

### Step 4: Generate Detailed Version

Target: 1,200-2,000 words. Analytical, causal, evidence-led. Every section has an editorial headline — a reframing or assertion, not a generic label.

Structure (from `templates/hiring-manager-detailed.md`):
1. Title and thesis — project title + one sentence naming the core PM challenge
2. Context — editorial headline, 2-3 paragraphs, closes with a reframe sentence before the problem section
3. Problem — editorial headline, numbered bold-header dimensions (3-4), each a distinct, nameable aspect
4. Users and Insights — headline IS the key finding; pull quote block if verbatim source material exists
5. My Role and Scope — editorial headline describing scope principle; bulleted ownership list
6. Strategy and Decision-Making — named decisions (bold headers) or decision table (4+ decisions)
7. Solution — editorial headline naming design principle; named elements by user/feature/component
8. Execution and Iteration — scope constraints, what changed mid-flight, iteration focus
9. Outcomes — stat blocks, interpretation paragraph, unexpected outcome paragraph if applicable
10. Reflection — pithy-statement-led paragraphs or two-part structure; mandatory closing statement

### Step 5: Generate Headlines and Subtitles

3 portfolio headline options — each from a different angle:
- Outcome angle: "[Metric] by [Action] — [Company] [Role]"
- Problem angle: "How I [Solved X Problem] for [User Type]"
- PM skill angle: "[Skill demonstrated] — [Project context]"

2 subtitle options — one punchy, one descriptive.

Up to 5 visual caption suggestions if the user mentioned screenshots, diagrams, or visuals.

### Step 6: Apply Writing Constraints

**This step is not optional.** Do not proceed to Step 7 until all 7 checks are resolved.

Scan the entire output:
- [ ] No em dashes anywhere in the body
- [ ] No bold markdown in the body (only in section headers is acceptable)
- [ ] No invented metrics or percentages
- [ ] No "I led" or "I owned" language unless it appeared in the user's source documents
- [ ] No placeholder text like "[insert X here]"
- [ ] Pre-launch status clearly stated if the project hasn't shipped
- [ ] No AI disclosure language in the output

### Step 7: Run Visual Layer

**This step always runs.** Do not skip it because the output looks complete.

Before saving, pass both generated texts and the project schema to `/visual-layer`.

The visual layer is a post-processing step only — it does not modify any factual content, tone, or generation logic. It adds three things:
1. A `visual_plan` JSON object with 4-8 recommended visuals
2. `[VISUAL: ...]` placeholders inserted into both case study texts at logical breakpoints
3. A `visual_captions` JSON object with one caption per visual

Load `prompts/visual-planning-prompt.md` and run its three prompts in sequence:
1. VISUAL PLANNING PROMPT → `visual_plan`
2. INSERT PLACEHOLDERS PROMPT → modified TLDR + modified detailed version
3. GENERATE CAPTIONS PROMPT → `visual_captions`

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "The evidence looks strong — I can skip Step 2's internal check" | Step 2 catches unsupported claims BEFORE they get written. Skipping it means weak causality or vague ownership embeds into the prose and is much harder to remove in polish. |
| "The output reads well — I can skip Step 6 constraint scan" | Em dashes and ownership inflation sound natural in prose. They only surface under deliberate scan. Step 6 must run on the complete output, not your memory of writing it. |
| "The case study is done — I can skip Step 7 visual layer" | Visual layer is structural, not decorative. It places [VISUAL: ...] anchors and produces the visual_plan JSON the portfolio site uses. Skipping it forces the user to re-run the whole skill. |
| "I can run Quality Check after saving" | Quality Check is a pre-save gate. Finding a missing sub-block after saving means the file is wrong. Run it before Step 8. |
| "The user named a project slug — I can skip the Pre-Generation Check" | The check verifies the file exists AND has the 4 required fields. A slug without a valid file means a hallucinated case study. |

## Before Marking Complete

Run this checklist before saving to `outputs/`:

- [ ] Step 0 files were read — name which project-inputs file was used
- [ ] Pre-Generation Check passed — all 4 required fields present
- [ ] TLDR has all 5 sub-blocks in correct sequence
- [ ] "What I Owned" is a numbered list (not bullets)
- [ ] Every section in detailed version has an editorial headline (assertion or reframe, not a generic label)
- [ ] Problem section has 3-4 numbered bold-header dimensions
- [ ] Users and Insights headline states the key finding (not "Users and Insights")
- [ ] Strategy section uses named decisions with alternatives OR a decision table
- [ ] Outcomes section has an interpretation paragraph (not just stat blocks)
- [ ] Reflection ends with a domain-specific generalizing closing statement
- [ ] Every claim is traceable to user-provided evidence — no invented metrics
- [ ] Step 6 constraint scan completed — no em dashes, no bold in prose paragraphs
- [ ] Step 7 visual layer ran — visual_plan JSON and [VISUAL: ...] placeholders are in the output

Only after all boxes checked: proceed to Step 8.

### Step 8: Save Output

Save to `outputs/[project-slug]-v1-[YYYY-MM-DD].md`.

Format:
```
# [Project Title] — Case Study

---

## RECRUITER TLDR

[350-500 words, with [VISUAL: ...] placeholders]

---

## DETAILED HIRING MANAGER VERSION

[1,200-2,000 words, with [VISUAL: ...] placeholders]

---

## PORTFOLIO ASSETS

Headlines:
1. ...
2. ...
3. ...

Subtitles:
1. ...
2. ...

---

## VISUAL PLAN

[visual_plan JSON, pretty-printed]

---

## VISUAL CAPTIONS

[visual_captions JSON, pretty-printed]
```

Tell the user: "Case study saved to `outputs/[filename]`. Visual plan and caption suggestions are included at the end of the file. Run `/polish` for a final quality pass, or `/evidence-check` to validate narrative angle and evidence strength before sharing."
