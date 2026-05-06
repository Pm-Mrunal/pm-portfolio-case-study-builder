# Hiring Manager Detailed Version — Output Template

> Target: 1,800-2,800 words. Analytical, causal, evidence-led. Every section earns its word count.
> Used by `/generate` as structural guidance.
> Every section has an editorial headline — an assertion or reframing, not a generic label.

---

## Section 1: Title and Thesis

[Project Title]
[One sentence: the core PM challenge — the tension, constraint, or design problem that made this interesting. Not a summary of what happened. The thesis names the specific difficulty.]

---

## Section 2: Context

**[Editorial headline — assertion that names the landscape situation. Example: "Two products that were never designed to coexist." / "When documentation becomes the bottleneck."]**

[Paragraph 1, 100-150 words: What existed before this project — the product landscape, user population, operational model, business context. Be specific: product names, price points, system names, user volumes where available.]

[Paragraph 2, 75-100 words: What was emerging or forcing action — the business mandate, market shift, technical capability, or internal pressure that made this project happen now.]

[Closing sentence: Reframe the real challenge before the Problem section names specific failures. Format: "The [challenge] was not [surface answer]. It was [actual challenge]." Narrows from landscape to problem without revealing the details yet.]

Length: 200-280 words.

---

## Section 3: Problem

**[Editorial headline — a short, bold assertion that reframes the problem. Not "The Problem." Not a question. Examples: "A business model problem masquerading as a portal feature." / "PDFs are not data."]**

[Opening sentence or two: Connect the context to the specific problem. Name the prior state.]

**1. [Short bold noun phrase — the label names the dimension]**
[1-2 sentences: who experienced this dimension, how it manifested, what the consequence was.]

**2. [Short bold noun phrase]**
[1-2 sentence explanation]

**3. [Short bold noun phrase]**
[1-2 sentence explanation]

**4. [Short bold noun phrase — optional, only if genuinely distinct from above]**
[1-2 sentence explanation]

[Optional closing sentence: What the numbered dimensions add up to — the systemic insight that connects them.]

Rules:
- Bold labels are noun phrases or short assertions — never questions
- Each dimension is distinct — not the same problem restated
- Maximum 4 numbered dimensions
Length: 200-300 words.

---

## Section 4: Users and Insights

**[Editorial headline — the key finding from discovery. Not "Users and Insights." Reads as a conclusion. Examples: "Accuracy mattered more than speed." / "The data was there. The synthesis wasn't."]**

[Paragraph 1, 75-100 words: Who the primary users were, the context in which they encountered the product, and the critical constraint or assumption that shaped everything that followed.]

[Paragraph 2, 75-125 words: The 2-3 most important insights from discovery, written causally. Lead with the finding, then what it changed. Do not list methods. Write what was learned and what decision it drove.]

[Pull quote block — only if verbatim quote or field observation is available in source data:]
> "[verbatim quote from source]"

Length: 150-250 words plus pull quote if available.

---

## Section 5: My Role and Scope

**[Editorial headline — scope principle in a phrase. Not "My Role and Scope." Examples: "Product definition through experience design." / "End-to-end product lead, pilot through stable handoff."]**

[Paragraph, 75-100 words: What you personally owned vs. what engineering, design, data science, or your manager owned. Name cross-functional partners. Be explicit — a hiring manager reads this for ownership clarity.]

- [Specific owned area — action phrase starting with a verb]
- [Another]
- [Another]
- [5-8 bullets total. Only use first-person ownership language where source data explicitly supports it.]

Length: 150-200 words.

---

## Section 6: Strategy and Decision-Making

**[Editorial headline — name the central strategic question or organizing principle. Examples: "Three decisions that shaped the product." / "The central question: when should AI act automatically?"]**

[Opening sentence: State the central strategic question or tension that organized the decisions.]

--- Option A: Named decisions (use for 2-4 decisions) ---

**[Decision name — short, bold, specific to this project]**
[2-4 sentences: what the options were, what was chosen, what tradeoff was accepted, and why. Reasoning chain must be explicit.]

**[Second decision name]**
[2-4 sentences]

**[Third decision name — if applicable]**
[2-4 sentences]

--- Option B: Decision table (use for 4+ decisions with clear alternatives) ---

| Decision | Choice | Rationale |
|---|---|---|
| [Area] | [What was chosen] | [1-2 sentence rationale] |
| [Area] | [What was chosen] | [Rationale] |

[Follow table with 1-2 paragraphs on the 1-2 most important decisions — what the pressure was, how you held the line, what you accepted.]

Rule: At least one decision must show alternatives explicitly considered and rejected.
Length: 300-450 words.

---

## Section 7: Solution

**[Editorial headline — names the design principle. Not "Solution." Examples: "A confidence-aware extraction pipeline." / "UX decisions from direct observation."]**

[Opening 1-2 sentences: Name the structural principle organizing the solution (by user type / by feature / by pipeline stage / by component). State the principle before naming the elements.]

**[Element name — bold noun phrase]**
[2-4 sentences: what it is, why it was designed this way, which problem, insight, or constraint it addresses. Explain reasoning, not just description.]

**[Second element name]**
[2-4 sentences]

**[Third element name — and so on]**
[2-4 sentences]

[Pull quote — only if a memorable first-person observation from source data is available:]
> "[verbatim statement from source]"

Breakdown principle selection:
- By user type: when different users have materially different experiences
- By feature or UX decision: when the most important choices were interface or workflow decisions
- By component or pipeline: when the solution is a system with distinct stages

Length: 300-400 words.

---

## Section 8: Execution and Iteration

**[Editorial headline — names the scope constraint or execution principle. Examples: "Scope discipline enabled a five-month MVP."]**

[100-200 words covering three things:]
- What bounded the MVP — which constraints shaped scope, which decisions were explicitly deferred and why
- What changed mid-flight — 1-2 things that were different from the plan and how they were handled
- Where iteration focused — not a list of sprints, but the 1-2 things that kept evolving and why

---

## Section 9: Outcomes

**[Outcomes — [subhead that frames the result. Examples: "What shipped and what happened." / "Measurable workflow impact."]]**

[Opening 1-2 sentences: What shipped and the overall result framing.]

[Stat blocks — 4-7 key metrics in this order:]
**[bold value]** [short label]
**[bold value]** [short label]
**[bold value]** [short label]
[Quantitative first. If using directional indicators for metrics without hard numbers, mark them clearly as directional.]

[Interpretation paragraph, 75-125 words: Draw a conclusion from the data together. What does the combination signal? What is the clearest proof the experience worked — not just that the feature shipped?]

[Unexpected outcome paragraph — only if source data documents a specific result that contradicted an assumption:]
One outcome was not anticipated. [Name it, explain it, and what it signals.]

Length: 200-300 words.

---

## Section 10: Reflection

**[Editorial headline — names the core learning or tension. Examples: "Designing the boundary between AI and clinician." / "What I learned and would do differently."]**

--- Option A: Pithy-statement-led paragraphs (use when 3+ distinct learnings are available) ---

[Short declarative statement — the lesson itself.] [2-4 sentences grounding it in a specific decision, moment, or data point from this project.]

[Second lesson statement.] [Elaboration grounded in this project.]

[Third lesson statement — if available.] [Elaboration.]

--- Option B: Two-part structure (use when 1-2 deep, specific learnings exist) ---

The hardest part of this project was not [the obvious answer]. It was [the actual challenge]. [2-3 sentences expanding.]

If I ran this project again, I would [specific action]. [The specific decision I made] led to [specific consequence]. [What I'd have done instead.]

An open question remains: [genuine unresolved tension — a real dilemma, not a rhetorical close.]

--- Mandatory closing statement (required for both options) ---

[1-2 sentences generalizing the core learning beyond this project. Specific enough to signal domain expertise. Examples: "In clinical environments, AI adoption depends less on model accuracy and more on product decisions that make uncertainty visible and controllable." / "Product decisions cannot be separated from context."]

Rules:
- Every named learning must be grounded in a specific decision, moment, or data point from this project
- Generic lessons that could apply to any PM on any project must be rewritten or cut
Length: 200-350 words.

---

> Generator notes — do not include in output:
> - No em dashes anywhere in prose. Verbatim quotes containing em dashes go in pull quote blocks only.
> - No bold markdown in prose paragraphs. Bold for numbered item labels and table headers only.
> - Each section must connect causally to the next — pull the reader forward
> - Reflection must name something specific — "I would have run a pricing test in parallel" not "I learned to involve stakeholders earlier"
> - If launch status is pre-launch, say so explicitly in Context and Outcomes
> - If role ownership used team language in source docs, use "contributed to" throughout
> - Do not include generator notes in the final output
