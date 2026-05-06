---
name: polish
description: Quality rewrite pass on a generated case study. Improves flow, clarity, scanability, and PM signal. Never adds new facts — only refines what's already there.
---

# /polish — Quality Rewrite Pass

## When to Use

- After `/generate` produces a draft
- User says "polish this" or "clean this up"
- Auto-triggered as the final step of `/quick-start`
- When a previously generated case study needs a refresh

## Inputs

- Generated case study draft (from `outputs/` or pasted directly)
- `prompts/polish-prompt.md`
- `context-library/writing-preferences.md` — auto-loaded for tone calibration

## What This Pass Does

Improves:
- Sentence flow and transitions
- Opening lines of each section
- PM signal strength (is the thinking visible?)
- Scanability of the TLDR
- Specificity of language (replace vague words with sharper ones)
- Causal connections (does insight visibly drive decision? Does decision visibly drive action?)
- Reflection quality (is it honest and specific, or generic?)

Does NOT:
- Add new facts
- Add invented metrics or outcomes
- Change role ownership language
- Inflate claims
- Alter the structure or sequence of sections

## Process

### Step 1: Load the Draft

Load the most recent draft from `outputs/` or accept a pasted draft.

### Step 2: Read for PM Signal

Before editing, assess:
- Is the PM's judgment visible throughout?
- Does the TLDR land in the first paragraph?
- Is every section earning its word count?
- Where does the narrative feel generic or flat?
- Is the reflection specific enough to differentiate a senior PM?

### Step 3: Rewrite Pass — TLDR

The TLDR has five labeled sub-blocks: At a glance / The Problem / Why This Matters / What I Owned / Key Results. Polish each:

- At a glance: must set context in one sentence without starting with "I" and without restating the title. If it reads as a summary of the case study rather than a context-setter, rewrite.
- The Problem: 2-3 sentences. Must lead with the problem — not the company, not the timeline. If the first word is the company name or a date, rewrite.
- Why This Matters: must read as structural insight, not a restatement of the problem. If it says the same thing as The Problem in different words, rewrite to identify the deeper reason this was worth solving.
- What I Owned: must be numbered items (not bullet points) beginning with verbs. Each item must be a specific area of ownership — not a generic job description phrase. "Led end-to-end product build from pilot through stable handoff" not "Led product development."
- Key Results: quantitative first, exact numbers from source. If no quant, rewrite qualitative results to be as concrete as possible. The most surprising or counterintuitive result goes last.

### Step 4: Rewrite Pass — Detailed Version

Focus on each section:

- Title and thesis: the thesis line (below the title) must name the specific PM challenge — the tension or constraint — not summarize what happened. If it reads as a description of the project outcome, rewrite.
- Context: must close with a sentence that reframes the real challenge before the Problem section names it. If Context ends with a neutral fact, add the reframe: "The challenge was not X. It was Y."
- Problem section: (a) the editorial headline must be an assertion, not a label — if it says "The Problem," rewrite it. (b) the numbered dimensions must each have a distinct, short bold label that names the dimension as a noun phrase. If labels are questions or overlap each other, rewrite. (c) the reader should feel the weight before the solution appears.
- Users and Insights: the section headline must state the key finding — not "Users and Insights." If the headline is generic, replace it with the single most important discovery. This section must lead with findings, not methods.
- Strategy and Decision-Making: the PM's reasoning chain must be explicit for each named decision. If a decision block describes only the choice with no alternatives or rejection rationale, strengthen it. If a decision table is present, each Rationale cell must explain the why, not just restate the choice.
- Solution: each named element must explain why it was designed the way it was — connecting back to a specific problem, insight, or constraint. If elements are described without rationale, add it using only information from the source data.
- Outcomes: the interpretation paragraph must draw a conclusion from the data together — what the combination signals, not just what each metric says individually. If the outcomes section is a list without this paragraph, add it.
- Reflection: the most common failure mode is generic learnings. If any paragraph could have been written by any PM on any project, rewrite it to be grounded in a specific decision, moment, or data point from this project. The closing statement must generalize beyond this project while remaining specific enough to signal domain expertise.

### Step 5: Language Sweep

Scan the full output and fix:
- Em dashes — remove all
- Bold markdown in body text — remove all
- Repeated sentence openings — vary them
- Passive voice where active is stronger
- Generic PM buzzwords without supporting specifics (e.g., "drove alignment," "unlocked growth," "leveraged synergies")
- Overlong introductory clauses — cut to the verb faster

### Step 6: Final Scan

Before saving:
- [ ] TLDR has all five labeled sub-blocks in the correct sequence
- [ ] "What I Owned" is a numbered list, not bullets
- [ ] "Why This Matters" reads as structural insight, not a restatement of the problem
- [ ] Every major section in the detailed version has an editorial headline (not a generic label)
- [ ] Problem section has 3-4 numbered bold-header dimensions with noun-phrase labels
- [ ] Users and Insights headline states the key finding
- [ ] Outcomes section has an interpretation paragraph (not just stat blocks)
- [ ] Reflection ends with a specific generalizing closing statement
- [ ] Detailed version tells a causal story from Context to Reflection
- [ ] No invented facts added during this pass
- [ ] Role ownership language unchanged from the original
- [ ] No em dashes in prose (verbatim pull quotes are exempt)
- [ ] No bold in prose paragraphs (numbered item labels and table headers are exempt)

### Step 7: Save

Increment the version number and save: `outputs/[project-slug]-v2-[YYYY-MM-DD].md`

Tell the user: "Polish complete. Saved to `outputs/[filename]`. Ready to share, or run `/review-as-recruiter` for a final critique."
