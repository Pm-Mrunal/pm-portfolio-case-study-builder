# Polish Prompt

> Used by the `/polish` skill for the final quality rewrite pass.
> No new facts are added. Only flow, clarity, and PM signal are improved.

---

## Prompt

```
Refine the case study draft below for publication quality.

Goals:
- Improve sentence flow and transitions
- Remove repetition and redundancy
- Sharpen scanability in the TLDR
- Strengthen PM signal throughout (make the PM's thinking more visible)
- Improve the causal chain: insight → decision → action → outcome
- Make the reflection more specific and honest (if it's generic, sharpen it)
- Preserve every factual claim exactly as written
- Do not add any new facts, metrics, or outcomes
- Do not exaggerate anything
- Keep the writing human and polished
- Keep role clarity precise — do not upgrade "contributed" to "owned"
- No em dashes
- No bold markdown in the body
- Do not use em dashes even if they appear in the draft

Specific things to fix if present:

TLDR:
- "At a glance" that starts with "I" or restates the project title → rewrite to set company/product context in one sentence
- "The Problem" that starts with the company name or timeline instead of the problem → rewrite to lead with what was broken
- "Why This Matters" that restates the problem in different words → rewrite to name the structural or strategic reason this was worth solving
- "What I Owned" that uses bullet points instead of numbered items → reformat as numbered list
- "What I Owned" items that are job description phrases → rewrite as specific PM action areas beginning with verbs
- "Key Results" that leads with qualitative before quantitative → reorder, quantitative first

Detailed version:
- Section headline that is a generic label ("Problem," "Solution," "Users and Insights") → rewrite as an editorial assertion or finding
- Problem section that describes the problem as a single blob without distinct numbered dimensions → restructure into 3-4 numbered bold-header dimensions
- Problem dimension labels that are questions or overlap each other → rewrite as distinct noun phrases
- Users and Insights headline that doesn't state the key finding → replace with the most important discovery
- Users and Insights that lists methods ("we conducted 18 interviews") → rewrite to lead with findings and connect each to the decision it drove
- Strategy section decision block that names only the choice without alternatives or rejection rationale → add the options considered and why they were rejected, using only information from source data
- Solution elements described without connecting to a specific problem, insight, or constraint → add the rationale using source data only
- Outcomes section that lists metrics without an interpretation paragraph → add 2-3 sentences drawing a conclusion from what the metrics signal together
- Reflection paragraph that could apply to any PM on any project → rewrite grounded in a specific decision, moment, or data point from this project
- Reflection that ends without a generalizing closing statement → add 1-2 sentences that generalize the core learning beyond this project
- Any sentence starting with "We" where first-person ownership is stated elsewhere → consider whether "I" is appropriate
- Any outcome described without causal attribution → add the connection if evidence supports it
- Repeated sentence openings across bullets or paragraphs → vary them

Make the draft feel like it was written by a thoughtful, senior product manager — not by a template or a generic assistant.

Draft:
{{DRAFT}}
```

---

## Polish Checklist

Run through this before saving the polished version:

**TLDR:**
- [ ] Five labeled sub-blocks present in the correct sequence (At a glance / The Problem / Why This Matters / What I Owned / Key Results)
- [ ] "At a glance" is one sentence setting context — does not start with "I" and does not restate the title
- [ ] "The Problem" leads with the problem, not the company or timeline
- [ ] "Why This Matters" reads as structural insight — not a restatement of the problem
- [ ] "What I Owned" is a numbered list with verb-led action phrases — not bullet points
- [ ] "Key Results" leads with quantitative, ends with the most surprising result if one exists

**Detailed version:**
- [ ] Every section has an editorial headline — no generic labels
- [ ] Problem section has 3-4 numbered bold-header dimensions with noun-phrase labels
- [ ] Users and Insights headline states the key finding (not "Users and Insights")
- [ ] Users and Insights leads with findings, not methods
- [ ] Strategy section shows alternatives considered and rejected for at least one decision
- [ ] Solution section names the structural breakdown principle before listing elements
- [ ] Outcomes section includes an interpretation paragraph (not just stat blocks)
- [ ] Reflection ends with a generalizing closing statement specific enough to signal domain expertise
- [ ] Each section connects causally to the next

**Language:**
- [ ] No em dashes in prose (verbatim pull quotes are exempt)
- [ ] No bold in prose paragraphs (numbered item labels and table headers are exempt)
- [ ] No "collaborated cross-functionally to drive alignment" without specifics
- [ ] No "significantly improved" without evidence
- [ ] No overlong introductory clauses

**Accuracy:**
- [ ] No new facts added
- [ ] No inflated claims introduced
- [ ] Role ownership language unchanged from the draft
