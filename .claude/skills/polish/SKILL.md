---
name: polish
description: "Runs a quality rewrite pass on a generated PM case study to improve flow, clarity, scanability, and PM signal without adding new facts. Use when the user says \"polish this\", \"clean this up\", \"tighten the reflection\", \"this feels flat\", \"sharpen the TLDR\", \"fix the flow\", \"make this shareable\", or asks to refresh a previously generated case study before sharing it. Do NOT use to add new content or sections → use /generate or /gap-detect instead. Do NOT use to validate evidence strength → use /evidence-check instead."
---

# /polish — Quality Rewrite Pass

## Step 0: Load Inputs

Load these files:

| Source | Path | What to extract |
|--------|------|-----------------|
| Case study draft | `outputs/[project-slug]-v[N]-[YYYY-MM-DD].md` | Full draft to rewrite |
| Writing preferences | `context-library/writing-preferences.md` | Tone, targeting, emphasis — load if exists |
| Polish prompt | `prompts/polish-prompt.md` | Additional rewrite guidance — load if exists |

**If no draft is specified and no slug is given:** List the most recent 3 output files found in `outputs/` and ask: "Which case study should I polish? I found these recent outputs: [list]. Or paste a draft directly."

Do not begin the rewrite until a specific draft is identified.

## Step 1: Read for PM Signal

Before editing, assess the draft and output a visible PM Signal Diagnostic:

For each section where PM judgment is implicit or language is generic, output:
- **Section:** [name]
- **Gap:** [what's implicit or generic]
- **Fix:** [what to make explicit — named alternative, causal connector, specific moment, or decision rationale]

At minimum, assess: TLDR "Why This Matters", Users & Insights headline, each decision in Strategy, Reflection closing paragraph. If a section passes ("PM judgment already explicit here"), say so and move on. Do not begin rewriting until the diagnostic is complete.

## Step 2: Rewrite Pass — TLDR

The TLDR has five labeled sub-blocks: At a glance / The Problem / Why This Matters / What I Owned / Key Results. Polish each:

- At a glance: must set context in one sentence without starting with "I" and without restating the title. If it reads as a summary of the case study rather than a context-setter, rewrite.
- The Problem: 2-3 sentences. Must lead with the problem — not the company, not the timeline. If the first word is the company name or a date, rewrite.
- Why This Matters: must read as structural insight, not a restatement of the problem. If it says the same thing as The Problem in different words, rewrite to identify the deeper reason this was worth solving.
- What I Owned: must be numbered items (not bullet points) beginning with verbs. Each item must be a specific area of ownership — not a generic job description phrase.
- Key Results: quantitative first, exact numbers from source. If no quant, rewrite qualitative results to be as concrete as possible. The most surprising or counterintuitive result goes last.

## Step 3: Rewrite Pass — Detailed Version

Focus on each section:

- Title and thesis: the thesis line must name the specific PM challenge — the tension or constraint — not summarize what happened. If it reads as a description of the project outcome, rewrite.
- Context: must close with a sentence that reframes the real challenge. If Context ends with a neutral fact, add the reframe: "The challenge was not X. It was Y."
- Problem section: (a) editorial headline must be an assertion, not a label — if it says "The Problem," rewrite it. (b) numbered dimensions must each have a distinct, short bold label as a noun phrase. (c) reader should feel the weight before the solution appears.
- Users and Insights: headline must state the key finding — not "Users and Insights." Replace generic headline with the single most important discovery. Lead with findings, not methods.
- Strategy and Decision-Making: PM's reasoning chain must be explicit for each named decision. If a decision block describes only the choice with no alternatives or rejection rationale, strengthen it. Each Rationale cell must explain the why, not restate the choice.
- Solution: each named element must explain why it was designed that way — connecting back to a specific problem, insight, or constraint. Use only information from the source data.
- Outcomes: interpretation paragraph must draw a conclusion from the data together — what the combination signals, not just what each metric says individually.
- Reflection: if any paragraph could have been written by any PM on any project, rewrite it grounded in a specific decision, moment, or data point from this project. Closing statement must generalize beyond this project while remaining specific enough to signal domain expertise.

## Step 4: Language Sweep

Scan the full output and fix:
- Em dashes — remove all (verbatim pull quotes are exempt)
- Bold markdown in body text — remove all (numbered item labels and table headers are exempt)
- Repeated sentence openings — vary them
- Passive voice where active is stronger
- Generic PM buzzwords without supporting specifics ("drove alignment," "unlocked growth," "leveraged synergies")
- Overlong introductory clauses — cut to the verb faster

## Step 5: Final Scan

Print this checklist with each item marked [x] (pass) or [ ] (fail). Do not save the output until all items are [x].

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
- [ ] No em dashes in prose
- [ ] No bold in prose paragraphs

If any item is [ ] (fail), fix it before saving.

## Step 6: Save

Increment the version number and save: `outputs/[project-slug]-v[N+1]-[YYYY-MM-DD].md`

Tell the user: "Polish complete. Saved to `outputs/[filename]`."

## Anti-Rationalization

| What you might think | Why it's wrong |
|----------------------|----------------|
| "The source draft is good enough — just do a light sweep" | Step 1 PM signal read identifies what's actually flat. Do the read first before deciding scope. |
| "I can skip the TLDR — the user only mentioned the detailed version" | The TLDR is the first thing a recruiter reads. If it's flat, it doesn't matter how good the body is. Always do Step 2. |
| "I'll add this section — the user implied it was missing" | /polish does not add new sections or facts. If something is missing, route to /gap-detect or /generate. |
| "The Final Scan checklist is just a formality — the rewrite was clean" | The checklist is a gate, not a formality. Print it with [x]/[ ] marks before saving. |
| "The user asked me to add technical details, so I'll add a quick section and polish the rest" | Out-of-scope request — decline the addition and explain why. Offer /gap-detect or /generate instead. Do not run an unsolicited polish pass. |

## Out of Scope

- Adding new facts, sections, or invented metrics → use `/generate` or re-run `/ingest` + `/extract` with richer source docs
- Identifying what's missing from the schema → use `/gap-detect`
- Validating whether claims are supported by evidence → use `/evidence-check`
- Assembling case studies into a portfolio website prompt → use `/build-portfolio`
