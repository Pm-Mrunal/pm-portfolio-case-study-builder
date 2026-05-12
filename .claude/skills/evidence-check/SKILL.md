---
name: evidence-check
description: Validates evidence strength before generating a PM case study. Scores each schema section (Problem, Role scope, Discovery, Strategy, Solution, Outcomes, Reflection, Pull quotes), flags risk patterns like overclaiming or ownership ambiguity, and recommends the 2–3 strongest narrative angles given available data. Use when the user says 'check my evidence', 'is my data strong enough', 'am I ready to generate', 'what's the strongest angle', 'I'm worried I'm overclaiming', 'my outcomes feel thin', 'my reflection feels weak', or before generating for senior roles (Staff PM, Director, VP). Do NOT use for identifying missing fields or filling gaps → use /gap-detect instead. Do NOT use to write or generate the case study → use /generate instead.
---

# /evidence-check — Pre-Generation Evidence Validation

## Step 0: Read Before You Write

| Source | Path | What to extract |
|--------|------|-----------------|
| Project schema | `context-library/project-inputs-[slug].md` | All 8 sections: Problem, Role scope, Discovery, Strategy/decisions, Solution, Outcomes, Reflection, Pull quotes |
| Evidence check prompt | `prompts/evidence-check-prompt.md` | Scoring criteria and risk flag definitions |

If the slug is not in context, list all `context-library/project-inputs-*.md` files and ask: "Which project should I run an evidence check on?" Do not proceed until the project is identified and the file is read.

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "Several sections look strong — I can skip scoring the weaker ones" | Score all 8 sections. Hiring managers read the whole case study. A weak Reflection after a strong Problem is still a gap the user needs to know about. |
| "The outcomes are clearly weak — I can skip the risk flag step" | Risk flags (Step 3) catch different problems than weakness ratings: overclaiming language, causality gaps, "we" ownership. Run Step 3 even when outcomes are already flagged weak. |
| "The user mentioned outcomes specifically — I'll focus there" | Score all sections first, then address the user's specific concern. Partial reports create false confidence in untouched sections. |
| "I have enough context from the conversation to skip reading the file" | Step 0 reads the actual project-inputs file. Conversation context may be incomplete or stale. Always read the file. |

## Process

### Step 1: Score Evidence Strength

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

## Cross-Skill Routing

After evidence check, route as follows based on what was found:

- Critical fields entirely absent (Outcomes = none, Role scope = none) → suggest running `/gap-detect` first to fill the highest-priority gaps before generating
- User says "generate anyway" after seeing weak evidence → proceed to `/generate` with the recommended narrative strategy applied
- All sections moderate or above, no critical risk flags → proceed directly to `/generate`
- User wants to fill gaps surfaced in the evidence check → route to `/gap-detect`

## Before Marking Complete

Do not consider this task finished until all of the following are true:

- [ ] Step 0 files were read — name which project-inputs file was used in your response
- [ ] All 8 schema sections scored with a strength rating and notes (Problem, Role scope, Discovery, Strategy/decisions, Solution, Outcomes, Reflection, Pull quotes)
- [ ] Step 3 risk flags checked — if none apply, state "No risk flags detected" explicitly
- [ ] At least 2 narrative angles recommended with project-specific rationale (not generic examples)
- [ ] Generation recommendation paragraph written — names what to emphasize, what safe phrasing to apply, what to watch for
- [ ] Step 6 offer made — asked whether user wants to generate or add more detail first

## Worked Example

**Input:** User has a project schema with strong problem and discovery, weak outcomes, and one verbatim user quote.

**Output shape:**

```
Evidence Check — [Project Title]

Strengths:
- [Specific field value or quote from the schema — e.g., "23% cart abandonment at 3x industry avg"]
- [Named insight from discovery — e.g., "3 named decisions with alternatives explicitly rejected"]
- [Human anchor — e.g., verbatim user quote: "I kept getting lost after I entered my card details"]

Gaps or weak areas:
- Outcomes: "Conversion improved significantly" — no number, no directional %, no proxy metric
- Reflection: Generic learning about engineering involvement — not grounded in a specific decision

Risk flags:
- "Improved significantly" is an overclaim without supporting data — use safe phrasing at generation time

Best narrative angles:
- Lead with the user quote as the case study anchor — it is the most human and specific element
- Frame as a high-judgment, tradeoff-driven case: 3 named decisions with rejected alternatives carry more weight than a missing outcome number

Generation recommendation:
Generate with discovery as the organizing spine. Use safe phrasing for outcomes: "Early validation showed..." or "Initial signals suggested..." — do not convert "improved significantly" into any number not provided by the user. For Reflection, ask the user to ground the learning in a specific moment before generating, or write it tied to the guest checkout tradeoff as the natural project-specific anchor.
```

## Notes

- This skill is informational — it does not alter the schema or generate any case study text.
- A weak evidence check result does not mean the case study will be bad. An honest, well-framed case study with limited outcomes can still be strong.
- Never tell the user their project "isn't good enough to write about." Every project has a case study in it — the skill is choosing the right angle.
