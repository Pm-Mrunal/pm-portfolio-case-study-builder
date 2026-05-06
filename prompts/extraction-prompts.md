# Extraction Prompts

> These are the per-field extraction prompts used by the `/extract` skill.
> Run each prompt against all document chunks for its target field.
> Return JSON for each field as specified.

---

## Problem Extraction

```
Extract the problem being solved from the text below.

Rules:
- Only extract explicitly stated problems — do not infer
- Include who is affected
- Include the prior state (what was happening before)
- Include why it mattered to the business or users
- Do not summarize vaguely — preserve specific language from the source

Return JSON:
{
  "problem_statement": "",
  "who": "",
  "prior_state": "",
  "why_it_mattered": "",
  "confidence": 0.0-1.0,
  "evidence": ["direct quote or close paraphrase 1", "direct quote 2"]
}

Text:
{{chunk}}
```

---

## Role and Ownership Extraction

```
Extract what this person personally owned, influenced, contributed to, and what others owned.

Rules:
- Distinguish between first-person language ("I built," "I led," "I owned") and team language ("we built," "the team shipped")
- First-person language = owned or led
- Team language without clarification = contributed or influenced
- If only team language exists, mark confidence low and flag for user clarification
- Extract cross-functional partners explicitly

Return JSON:
{
  "title": "",
  "what_i_owned": [],
  "what_i_influenced": [],
  "what_others_owned": [],
  "cross_functional_partners": [],
  "team_composition": "",
  "confidence": 0.0-1.0,
  "ownership_clarity": "explicit | partial | unclear",
  "evidence": ["quote 1", "quote 2"]
}

Text:
{{chunk}}
```

---

## Outcomes Extraction

```
Extract all outcome data from the text below.

Rules:
- Extract quantitative results first (percentages, absolute numbers, rates, revenue)
- Then qualitative results (user feedback, team observations, operational changes)
- Then validation signals (cohort data, A/B test results, directional observations)
- Never infer outcomes — only extract what is explicitly stated
- Preserve exact numbers — do not round or approximate

Return JSON:
{
  "launch_status": "shipped | pilot | pre-launch | prototype | unknown",
  "quantitative_results": [],
  "qualitative_results": [],
  "validation_signals": [],
  "business_impact": [],
  "user_impact": [],
  "confidence": 0.0-1.0,
  "evidence": ["quote 1", "quote 2"]
}

Text:
{{chunk}}
```

---

## Discovery and Insights Extraction

```
Extract user research, discovery methods, and insights from the text below.

Rules:
- Distinguish between methods (what was done) and insights (what was learned)
- A method is: "we ran 18 interviews"
- An insight is: "users dropped at the data connection step because they didn't have API keys ready"
- Insights that changed the approach are higher value — flag these
- Only extract explicitly stated findings — do not generate insights

Return JSON:
{
  "research_methods": [],
  "top_insights": [],
  "assumptions_that_changed": [],
  "evidence_points": [],
  "confidence": 0.0-1.0,
  "evidence": ["quote 1", "quote 2"]
}

Text:
{{chunk}}
```

---

## Strategy and Decisions Extraction

```
Extract strategic decisions, options considered, and tradeoffs from the text below.

Rules:
- Look for explicit alternatives that were evaluated (even if briefly mentioned)
- Look for rejection rationale (why was option X not chosen?)
- Look for tradeoffs explicitly accepted (we accepted X risk because Y)
- Do not infer strategic intent — only extract stated decisions
- A decision without alternatives is lower value — flag it

Return JSON:
{
  "options_considered": [],
  "selected_direction": "",
  "tradeoffs": [],
  "decision_rationale": "",
  "confidence": 0.0-1.0,
  "tradeoff_explicitness": "explicit | implied | absent",
  "evidence": ["quote 1", "quote 2"]
}

Text:
{{chunk}}
```

---

## Solution Extraction

```
Extract the solution, key changes, and scope boundaries from the text below.

Rules:
- Extract what specifically changed or was built
- Extract scope boundaries (what was NOT included)
- Extract rollout or launch approach if mentioned
- Preserve specific feature names, system names, and process changes

Return JSON:
{
  "solution_summary": "",
  "key_changes": [],
  "scope_boundaries": [],
  "rollout_scope": "",
  "confidence": 0.0-1.0,
  "evidence": ["quote 1", "quote 2"]
}

Text:
{{chunk}}
```

---

## Reflection Extraction

```
Extract learnings, what the person would do differently, and open questions from the text below.

Rules:
- A strong learning is specific to this project — not a generic PM lesson
- "What I would do differently" should name a specific action, not a vague intention
- Open questions should be real unresolved questions, not rhetorical closings
- If the reflection is generic, mark confidence low

Return JSON:
{
  "key_learnings": [],
  "what_i_would_do_differently": [],
  "open_questions": [],
  "what_this_demonstrates": [],
  "confidence": 0.0-1.0,
  "reflection_quality": "specific | partial | generic",
  "evidence": ["quote 1", "quote 2"]
}

Text:
{{chunk}}
```

---

## Pull Quote Extraction

```
Extract verbatim quotable statements from the text below.

A pull quote is a verbatim statement that crystallizes a key insight, decision, or constraint. It can be:
- A direct quote from a user, customer, or field staff member
- A memorable first-person observation by the PM ("You cannot build a tool for frontline people who use it in the sun while sitting in an AC room far away.")
- A verbatim customer verbatim from research, cancellation data, support tickets, or feedback channels
- A framing statement that captures the core tension of the project in one sentence

Rules:
- Only extract verbatim or near-verbatim language from the source — do not paraphrase into pull quote form
- A pull quote must be self-contained — a reader should understand its point without surrounding context
- Short is better: 10-30 words is ideal, 50 words is the maximum
- Do not generate a pull quote if no strong verbatim candidate exists in the source

Return JSON:
{
  "pull_quotes": [
    {
      "text": "verbatim quote",
      "source": "user | customer | field_staff | pm_observation | feedback_data | other",
      "section_fit": "problem | discovery | solution | reflection",
      "confidence": "high | medium | low"
    }
  ],
  "pull_quote_available": true | false,
  "evidence": ["source text where quote appears"]
}

Text:
{{chunk}}
```

---

## Named Decisions Extraction

```
Extract individual strategic and product decisions as named, structured items from the text below.

Rules:
- Each decision must be a discrete, nameable choice — not a general approach
- Only extract decisions where there is evidence of alternatives considered or tradeoffs accepted
- A decision without alternatives or rationale is lower value — mark it accordingly
- Decision names should be short noun phrases specific to this project ("Cutting the Premium tier" not "Scope decision")
- Do not infer decisions — only extract explicitly stated ones

Return JSON:
{
  "named_decisions": [
    {
      "name": "short noun phrase naming the decision",
      "options_considered": ["option 1", "option 2"],
      "choice_made": "what was selected",
      "rationale": "why this was chosen",
      "tradeoff_accepted": "what was given up or risked",
      "decision_quality": "explicit_tradeoff | choice_only | vague",
      "confidence": "high | medium | low",
      "evidence": ["supporting quote"]
    }
  ],
  "decision_count": 0,
  "table_viable": true | false,
  "evidence": ["source quote 1"]
}

Note: Set table_viable to true if 4 or more named decisions with clear alternatives are extractable.

Text:
{{chunk}}
```

---

## Solution Breakdown Extraction

```
Extract information about how the solution is organized and what structural principle best organizes its presentation.

Rules:
- A solution organized around different user types should be presented by user type
- A solution organized around feature decisions should be presented by feature
- A solution organized around technical stages should be presented by pipeline component
- Do not force a breakdown type — extract what is natural given the source

Return JSON:
{
  "solution_elements": [
    {
      "name": "short noun phrase naming this element",
      "element_type": "user_type | feature | pipeline_stage | concept",
      "description": "what this element does",
      "design_rationale": "why it was designed this way",
      "problem_addressed": "which specific problem or insight this addresses",
      "confidence": "high | medium | low"
    }
  ],
  "recommended_breakdown": "by_user_type | by_feature | by_component | by_concept",
  "breakdown_rationale": "brief reason for this recommendation",
  "evidence": ["supporting quote"]
}

Text:
{{chunk}}
```
