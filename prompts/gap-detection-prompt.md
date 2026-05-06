# Gap Detection Prompt

> Used by the `/gap-detect` skill to identify missing fields and generate targeted questions.

---

## Prompt

```
You are reviewing structured PM case study data.

Your job is to identify the most critical missing inputs — the gaps that would most significantly weaken the case study or prevent it from matching the quality of a top-tier PM portfolio.

Input schema:
{{PROJECT_SCHEMA_JSON}}

Evaluate gaps in this priority order:

---

## CRITICAL GAPS (always ask if missing)

**1. ROLE OWNERSHIP**
Is it clear what this PM personally owned vs. contributed to vs. supported? If the schema has only team-level language ("we built," "the team shipped") with no first-person ownership, this is critical. A case study with ambiguous ownership fails with hiring managers before they finish reading.

**2. PROBLEM CLARITY**
Is the problem articulated with a prior state, an affected user or user type, and business stakes? A vague problem statement ("we needed to improve the product") is a critical gap. The problem section will use 3-4 numbered dimensions — if only 1-2 distinct problem dimensions are present, the section will feel thin.

**3. OUTCOMES**
Is there any quantitative result, directional signal, or validated observation? A complete absence of outcomes is critical. Note: absence of hard metrics is NOT critical if there are clear validated signals or delivered scope that can stand in.

**4. KEY DECISIONS WITH ALTERNATIVES**
Is there at least one strategic or product decision where alternatives were explicitly considered and rejected? A case study with no visible decision-making — just the chosen direction — demonstrates no PM judgment. This is a critical gap.

---

## HIGH-VALUE GAPS (ask if 1-2 slots remain after critical gaps)

**5. NAMED DECISIONS (minimum 2)**
Are there 2 or more distinctly named decisions with rationale? If only 1 named decision exists, the Strategy section will be weak. If 4+ named decisions exist, a decision table becomes possible — flag this if more decisions could be surfaced. Ask: "What other product or strategic calls did you make on this project — even smaller ones like what to defer, how to frame the narrative, or which users to prioritize?"

**6. PULL QUOTE MATERIAL**
Is there at least one verbatim user quote, customer verbatim, field observation, or memorable PM statement in the source data? Pull quotes ground the case study in a human moment. If absent, ask: "Do you have any direct quotes from users, customers, or field staff — even from cancellation feedback, support tickets, or research sessions? Or a specific field observation that crystallized the problem for you?"

**7. NAMED DISCOVERY INSIGHTS**
Are discovery insights labeled with memorable, specific findings — not just listed as prose? Insights like "Sunlight broke our usability assumptions" or "Accuracy mattered more than speed" are what make the Users section headline work. If insights are only described in prose, ask: "What is the most surprising or counterintuitive thing you learned during discovery — something that changed how you thought about the problem?"

**8. SOLUTION BREAKDOWN TYPE**
Is it clear what structural principle should organize the Solution section — by user type, by feature, or by pipeline component? If the source only describes the solution as a unified product without distinct elements, ask: "Walk me through the 2-4 most important parts of what was built or changed. For each one: what was the key design decision, and why was it designed that way rather than another way?"

---

## LOWER-PRIORITY GAPS (ask only if nothing critical or high-value is missing)

- Specific "what I'd do differently": name one decision, its consequence, what they'd have done instead
- Team composition specifics beyond what's already captured
- Exact timeline dates (if approximate dates exist, this is fine)
- Secondary outcomes or validation signals

---

## Rules

- Return a MAXIMUM of 5 questions
- Only ask about gaps that would materially improve the case study
- If all critical and high-value areas are reasonably covered, ask only 2-3 questions about the weakest areas
- Write questions in plain language — specific enough that the user can answer concisely
- Give the user explicit permission to be imprecise ("even a directional estimate helps," "rough language counts")
- For questions about pull quotes: tell the user that even a paraphrase of what someone said counts

Return JSON:
{
  "gap_summary": "2-sentence summary of the most important gaps and what they would fix",
  "questions": [
    {
      "field": "field_name",
      "priority": "critical | high | medium",
      "question": "The actual question — specific, not generic",
      "why_it_matters": "One sentence on what this gap costs the case study",
      "permission_phrase": "The phrase that gives the user permission to be imprecise"
    }
  ],
  "generation_viable": true | false,
  "generation_note": "If false: the minimum information needed before generation can proceed."
}
```

---

## Gap Question Writing Guide

Use these as examples of strong vs. weak gap questions:

**Role ownership:**
Strong: "What did you personally own on this project — discovery, the roadmap, stakeholder comms, experiment design? And what was owned by others like engineering, design, or your manager?"
Weak: "What was your role?"

**Outcomes:**
Strong: "What's the strongest outcome you can point to — even if it's directional? A conversion rate shift, a reduction in support tickets, a qualitative signal from users or stakeholders, or a metric that moved even a little?"
Weak: "What were the results?"

**Named decisions:**
Strong: "What was the most important product or strategic call you made on this project — something you could have decided differently? What were the alternatives, and why did you make the call you did? And were there 1-2 other calls like that — things you cut, deferred, or reframed?"
Weak: "What decisions did you make?"

**Pull quote:**
Strong: "Do you have any direct quotes from users, customers, or field staff — even rough paraphrases from research, cancellation reasons, or support tickets? Or is there a specific thing you observed in the field or a conversation that crystallized the problem for you?"
Weak: "Do you have any user quotes?"

**Solution breakdown:**
Strong: "Walk me through the 2-4 most important parts of what was built or changed. For each one: what problem or insight drove that specific design decision, and what was the alternative you considered and rejected?"
Weak: "Can you describe the solution?"

**Discovery insight:**
Strong: "What is the most surprising or counterintuitive thing you learned during discovery — something that changed how you thought about the problem or the user? Something you assumed going in that turned out to be wrong?"
Weak: "What did you learn from users?"
