---
name: generate
description: Generate the recruiter TLDR and detailed hiring manager case study versions from the structured schema. Uses the generation prompt and output templates.
---

# /generate — Case Study Generation

## When to Use

- After `/gap-detect` has collected answers
- User says "generate my case study" or "write it now"
- Auto-triggered by `/quick-start` after gap questions are answered
- User has manually filled `context-library/project-inputs-[slug].md` and is ready to generate
- User says `/generate [project-slug]` to generate a specific existing project

## Inputs

- `context-library/project-inputs-[slug].md` — populated schema for the active project (required)
- `context-library/writing-preferences.md` — tone and targeting (auto-loaded if filled)
- `prompts/generation-prompt.md` — the core generation instructions
- `templates/recruiter-tldr.md` — TLDR structural template
- `templates/hiring-manager-detailed.md` — detailed version structural template

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

Target: 1,800-2,800 words. Analytical, causal, evidence-led. Every section has an editorial headline — a reframing or assertion, not a generic label.

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

Scan the entire output before saving:
- [ ] No em dashes anywhere in the body
- [ ] No bold markdown in the body (only in section headers is acceptable)
- [ ] No invented metrics or percentages
- [ ] No "I led" or "I owned" language unless it appeared in the user's source documents
- [ ] No placeholder text like "[insert X here]"
- [ ] Pre-launch status clearly stated if the project hasn't shipped
- [ ] No AI disclosure language in the output

### Step 7: Run Visual Layer

Before saving, pass both generated texts and the project schema to `/visual-layer`.

The visual layer is a post-processing step only — it does not modify any factual content, tone, or generation logic. It adds three things:
1. A `visual_plan` JSON object with 4-8 recommended visuals
2. `[VISUAL: ...]` placeholders inserted into both case study texts at logical breakpoints
3. A `visual_captions` JSON object with one caption per visual

Load `prompts/visual-planning-prompt.md` and run its three prompts in sequence:
1. VISUAL PLANNING PROMPT → `visual_plan`
2. INSERT PLACEHOLDERS PROMPT → modified TLDR + modified detailed version
3. GENERATE CAPTIONS PROMPT → `visual_captions`

This step is always run. It does not require user input.

### Step 8: Save Output

Save to `outputs/[project-slug]-v1-[YYYY-MM-DD].md`.

Format:
```
# [Project Title] — Case Study

---

## RECRUITER TLDR

[450-650 words, with [VISUAL: ...] placeholders]

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

Tell the user: "Case study saved to `outputs/[filename]`. Visual plan and caption suggestions are included at the end of the file. Run `/polish` for a final quality pass, or `/review-as-recruiter` and `/review-as-hiring-manager` for critique."

## Quality Check

After generating, review:
- Does the TLDR use all five labeled sub-blocks in the correct sequence?
- Is "What I Owned" a numbered list (not bullets)?
- Does every major section in the detailed version have an editorial headline — not a generic label?
- Does the Problem section have 3-4 numbered bold-header dimensions?
- Does the Users and Insights headline state the key finding (not "Users and Insights")?
- Does the Strategy section use named decisions with alternatives, or a decision table?
- Does the Solution section name the structural breakdown principle before listing elements?
- Does the Outcomes section have an interpretation paragraph (not just stat blocks)?
- Does the Reflection end with a generalizing closing statement specific enough to signal domain expertise?
- Is every claim traceable to user-provided evidence?
- No em dashes in prose? No bold in prose paragraphs?
- Would a reader know exactly what this PM personally owned vs. contributed to?
