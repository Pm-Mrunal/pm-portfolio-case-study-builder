# Evidence Check Prompt

> Used by the `/evidence-check` skill before generation.
> Validates evidence strength and recommends the best narrative angle.

---

## Prompt

```
You are validating structured inputs for a PM portfolio case study generator.

Review the project schema below and assess whether it has enough evidence to generate a credible portfolio case study.

Return your answer in JSON with these keys:

- strengths: array of the 2-4 strongest available proof points (be specific — quote actual data points from the schema)
- missing_or_weak: array of important gaps with a brief note on why each matters for case study quality. Specifically evaluate:
    - Problem dimensions: are there 3-4 distinct, nameable problem aspects, or is the problem described as one undifferentiated blob?
    - Named decisions: are there at least 2 distinct decisions with alternatives explicitly considered and rejected?
    - Pull quote material: is there any verbatim user quote, customer verbatim, field observation, or memorable PM statement?
    - Solution breakdown: is it clear what structural principle organizes the solution (by user type / by feature / by component)?
    - Reflection specificity: is there one "what I'd do differently" grounded in a specific decision and consequence?
- risk_flags: array of any claims that may be unsupported or overstated — these need careful handling in generation
- best_angles: array of the 2-3 strongest narrative angles to emphasize given what's available
- output_strategy: object with:
    - recruiter_focus: what to emphasize in the TLDR given this evidence profile
    - hiring_manager_focus: what to emphasize in the detailed version
    - evidence_limitations: honest note on what the case study won't be able to claim
- recommended_cautions: array of specific wording cautions for the generator (e.g., "do not imply the product launched" / "role ownership is team-level — use contributed language" / "no pull quote material available — skip pull quote blocks" / "only 1 named decision — cannot use decision table format, use single named-decision block")

Rules:
- Do not invent data
- Do not rewrite the case study yet
- Focus on: ownership clarity, insight quality, decision quality, outcome quality, and credibility
- Explicitly assess: named decision count, pull quote availability, solution breakdown clarity
- If metrics are missing, say so
- If role boundaries are unclear, say so
- If the story is stronger as a learning case, pivot case, 0-to-1 case, or platform case — say so

Project schema:
{{PROJECT_SCHEMA_JSON}}
```

---

## Narrative Angle Guide

Use these to classify the best angle for each case study type:

**Strong metrics available:**
Lead with outcomes. Make the TLDR results-first. Frame the detailed version around "how we got there."

**No metrics, strong decision-making:**
Lead with judgment. Frame as a high-ambiguity case where strategic choices drove the outcome, even if the outcome is pre-launch or directional.

**0-to-1 project:**
Lead with the blank slate. Show how you moved from nothing to a shipped (or validated) product. Emphasize problem definition and the choices that shaped the MVP.

**Redesign or iteration:**
Lead with prior state contrast. What existed before? What was broken? The "what changed and why" narrative is the case.

**No launch / prototype:**
Lead with what was learned. An honest "we validated X and decided Y" is more credible than inflated claims. Show how the work informed what came next.

**Internal tool or ops project:**
Lead with the operational problem. These cases often have clear before/after comparisons. Efficiency gains and team impact are valid outcomes.

**AI or technical project:**
Lead with the PM angle, not the tech. Show the product judgment embedded in the technical decisions — what you chose to build vs. not build, and why.
