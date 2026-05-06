---
name: evidence-check
description: Validate evidence strength before generation. Identifies what's strong, what's weak, what might be unsupported, and recommends the best narrative angle given available data.
---

# /evidence-check — Pre-Generation Evidence Validation

## When to Use

- Before running `/generate` on a complex or high-stakes case study
- User says "check my evidence" or "what's the strongest angle?"
- When the extracted schema feels thin and you want to understand what you're working with
- Recommended before generating for senior roles (Staff PM, Director, VP)

## Inputs

- `context-library/project-inputs-[slug].md` — populated schema for the active project
- `prompts/evidence-check-prompt.md`

## Process

### Step 1: Load Schema

Read the full `context-library/project-inputs-[slug].md` for the active project. If the slug is not in context, list all `context-library/project-inputs-*.md` files and ask which project to check. Assess each section.

### Step 2: Score Evidence Strength

Rate each major section:

| Section | Strength | Notes |
|---|---|---|
| Problem | strong / moderate / weak | Clear prior state, affected user, and business stakes? At least 3 distinct, nameable problem dimensions? |
| Role scope | strong / moderate / weak | Ownership explicit? Lines between owned/influenced/supported clear? |
| Discovery | strong / moderate / weak | Actual named insights (not just methods)? Any verbatim user quotes or field observations? |
| Strategy/decisions | strong / moderate / weak | At least 2 named decisions with alternatives explicitly considered and rejected? Tradeoffs stated? |
| Solution | strong / moderate / weak | Organized by a structural principle (by user / by feature / by component)? Each element connected to a problem or insight? |
| Outcomes | strong / moderate / weak | Quantitative? Directional? Do the metrics support an interpretation paragraph (what they signal together)? |
| Reflection | strong / moderate / weak | Specific named learnings? One "what I'd do differently" grounded in a specific decision? A generalizing closing statement? |
| Pull quote material | present / absent | Any verbatim user quotes, customer verbatims, field observations, or memorable PM statements? |

### Step 3: Identify Risk Flags

Flag any of these:
- **Ownership ambiguity**: "we" language throughout with no first-person ownership statements
- **Outcome vacuum**: no quantitative results, no directional signals, no validation evidence
- **Causality gap**: solution described but no connection to the problem or insights
- **Overstated claims**: language suggesting outcomes that aren't supported ("drove significant growth," "transformed the product")
- **Missing tradeoffs**: only the chosen direction described, no alternatives or rejection rationale — Strategy section will not demonstrate PM judgment
- **No named decisions**: if fewer than 2 distinct decisions with named alternatives exist, the Strategy section will be weak regardless of format
- **No pull quote material**: the case study will lack any human voice anchor — consider asking the user for a verbatim user quote or field observation before generating
- **Solution undifferentiated**: solution described as a single unified product without distinct named elements — the Solution section will default to prose and miss structural depth
- **Generic reflection**: learnings that could apply to any PM on any project, with no project-specific grounding

### Step 4: Identify Best Narrative Angles

Based on what IS strong, recommend the 2-3 best angles for generation:

Examples:
- "Your discovery section is the strongest part — lead with the insight that changed your direction."
- "You have no metrics but strong tradeoff documentation — frame this as a high-judgment, high-ambiguity case."
- "Your 0-to-1 arc is clear — emphasize builder narrative and scope delivered."
- "Your reflection is unusually honest — use it to signal senior maturity."

### Step 5: Output Report

Present a concise evidence report to the user:

```
Evidence Check — [Project Title]

Strengths:
- [Strongest proof point 1]
- [Strongest proof point 2]
- [Strongest proof point 3]

Gaps or weak areas:
- [Gap 1 with brief note]
- [Gap 2]

Risk flags:
- [Any overclaims or unsupported assertions]

Best narrative angles:
- [Angle 1 — what to lead with]
- [Angle 2 — secondary framing]

Generation recommendation:
[One paragraph summarizing how to approach generation given this evidence profile — what to emphasize, what to handle carefully, what safe phrasing to use]
```

### Step 6: Offer to Proceed

After the report, ask: "Ready to generate? Or would you like to add more detail to any of the weak areas first?"

If the user wants to add more, accept new input and re-run extraction before generating.
If the user says "generate anyway," proceed to `/generate` with the current schema and apply the recommended narrative strategy.

## Notes

- This skill is informational — it does not alter the schema or generate any case study text.
- A weak evidence check result doesn't mean the case study will be bad. A honest, well-framed case study with limited outcomes can still be strong.
- Never tell the user their project "isn't good enough to write about." Every project has a case study in it — the skill is choosing the right angle.
