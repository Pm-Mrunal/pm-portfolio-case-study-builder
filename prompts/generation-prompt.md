# Generation Prompt

> The core prompt used by the `/generate` skill.
> Apply `prompts/system-prompt.md` as the system context before running this.

---

## What You Are Generating

Produce two portfolio case study versions from the structured project data below:

1. A recruiter TLDR (350-500 words of content, five labeled sub-blocks)
2. A detailed hiring manager version (1,800-2,800 words, ten structured sections)

These are not conventional PM case studies. They follow an editorial genre — structured documents that demonstrate analytical rigor through named sections, deliberate compositional moves, and visual organization. Follow every format instruction exactly.

Use only the facts provided. Do not invent or embellish. If evidence is limited, write truthfully and make the story as strong as the available evidence allows.

---

## RECRUITER TLDR

Format: Five labeled sub-blocks in this exact sequence. Use the label names exactly as written below.

```
[Project Title]
[Company] | [Role] | [Timeline] | [Stage: Shipped / Pilot / Pre-launch]

---

At a glance
[One sentence. Set the company/product context and name the project. Do not start with "I." Do not repeat the project title. Example: "Vantage is a B2B SaaS spend management platform. This project redesigned the self-serve onboarding flow to remove friction at the critical activation step, increasing 7-day activation by 22% and reducing CS onboarding calls by 40%."]

---

The Problem
[2-3 sentences. Who experienced the problem, what the prior state was, and what the business stakes were. Lead with the problem — not the company and not a summary. Make the reader feel the weight of what was broken before the next sub-block explains the stakes.]

---

Why This Matters
[1-2 sentences. The structural or strategic reason this problem was worth solving — not just the symptoms. The "so what" for a reader evaluating PM judgment. Should read as insight, not recap. Example: "The separation wasn't just inefficient — it was blocking price-sensitive customers from accessing a service they needed and creating structural debt the business couldn't collect."]

---

What I Owned
1. [PM action phrase beginning with a verb: Led / Defined / Designed / Cut / Ran / Partnered with / Drove / Established]
2. [Another specific ownership area]
3. [Another]
4. [Another]
5. [Another]
6. [Optional sixth — only if genuinely distinct from above]
7. [Optional seventh]

[Use numbered items, not bullet points. Only use "Led" or "Owned" if source data explicitly states first-person leadership. Default to "Contributed to," "Partnered with," or "Supported" where ownership is shared or unclear.]

---

Key Results
- [Most compelling quantitative result — exact numbers from source data, no rounding]
- [Second result — a different dimension from the first: user, operational, financial, or behavioral]
- [Third result]
- [Fourth result if available]
- [Optional fifth — the most surprising or counterintuitive result if one exists]

[Do not introduce results that are not in the source data. If metrics are absent, use directional signals phrased precisely: "bad debt eliminated for portal Standard bookings" not "bad debt significantly reduced."]
```

---

## DETAILED HIRING MANAGER VERSION

Target: 1,800-2,800 words. Write every section to earn its word count. If a section has nothing specific to say, shorten it — do not pad.

### Section Format Rules (apply to every section)

**Editorial headlines:** Every major section has an editorial headline — a reframing, assertion, or insight, not a description of what the section contains. The headline is the most important compositional choice in each section.

Wrong: "Problem"
Right: "A business model problem masquerading as a portal feature"

Wrong: "Users and Insights"
Right: "Accuracy mattered more than speed"

Exceptions where descriptive headlines are acceptable: My Role and Scope, Execution, Outcomes (subhead only).

**Em dash rule:** No em dashes in generated prose. If source data contains a verbatim quote with an em dash, render it as a pull quote block — indented, set apart from prose — not woven into prose sentences.

**Bold rule:** No bold markdown in prose paragraphs. Bold is allowed only for: (a) numbered item labels in structured analysis blocks, (b) decision table column headers.

**Causal chain rule:** Each section's closing sentence should create momentum into the next. The reader should feel pulled forward, not stopped.

---

### Section 1: Title and Thesis

**Format:**
Line 1: Project title
Line 2: One sentence capturing the core PM challenge — the tension, constraint, or design problem that made this project interesting. Not a summary of what happened. The thesis names the specific difficulty. Example: "Designing AI-assisted biomarker extraction clinicians could trust — balancing automation with clinical safety in high-stakes documentation workflows."

---

### Section 2: Context

**Editorial headline:** A short assertion or reframing that names the landscape situation before the problem is named. Example: "When documentation becomes the bottleneck" / "Two products that were never designed to coexist."

**Format:**
- Paragraph 1 (100-150 words): What existed before this project — the product landscape, the user population, the operational model, the business context. Be specific: product names, price points, system names, user volumes where available.
- Paragraph 2 (75-100 words): What was emerging, changing, or forcing action — the business mandate, the market shift, the technical capability, or the internal pressure that made this project happen now.
- Closing sentence: Reframe the real challenge before the Problem section names the specific failures. Format: "The [challenge] was not [surface answer]. It was [actual challenge]." This sentence narrows the frame from landscape to problem without revealing the problem details yet.

**Length:** 200-280 words.

---

### Section 3: Problem

**Editorial headline:** A short, bold assertion that reframes the problem — not "The Problem," not a question. Something the reader would not have said themselves. Examples: "A business model problem masquerading as a portal feature." / "PDFs are not data." / "The access gap was structural, not clinical."

**Format:**
- Opening sentence or two: Connect the context to the specific problem. Name the prior state.
- Then: 3-4 numbered bold-header problem dimensions. Format exactly as shown:

**1. [Short bold noun phrase — the label names the dimension, not the explanation]**
[1-2 sentences specific to this project: who experienced this dimension, how it manifested, what the consequence was.]

**2. [Short bold noun phrase]**
[1-2 sentence explanation]

**3. [Short bold noun phrase]**
[1-2 sentence explanation]

**4. [Short bold noun phrase — optional, only include if genuinely distinct from the above]**
[1-2 sentence explanation]

- Optional closing sentence: What the numbered dimensions add up to — the systemic or structural insight that connects them.

**Rules:**
- The bold label is always a noun phrase or short assertion. Never a question. "Customer confusion" not "Why were customers confused?"
- Each dimension must be a distinct, nameable aspect of the problem — not the same problem restated.
- Do not number beyond 4. If more than 4 dimensions exist, merge the weakest two.

**Length:** 200-300 words.

---

### Section 4: Users and Insights

**Editorial headline:** The headline IS the key finding from discovery — the most important thing learned. Not "Users and Insights." Read as a conclusion, not a topic label. Examples: "Accuracy mattered more than speed." / "The data was there. The synthesis wasn't." / "Physicians were never the bottleneck."

**Format:**
- Paragraph 1 (75-100 words): Who the primary users were, the context in which they encountered the product, and the critical constraint or assumption that shaped everything that followed.
- Paragraph 2 (75-125 words): The 2-3 most important insights from discovery, written causally. Lead with the finding, then what it changed. Do not list methods. Do not write "we ran interviews." Write what was learned and what decision it drove.

**Pull quote:** If the source data contains a verbatim user quote, customer verbatim, or field observation, render it as a pull quote block after this section. Use the exact verbatim language from the source. Do not generate a quote if one is not in the source data.

> "[verbatim quote from source data]"

**Length:** 150-250 words plus pull quote if available.

---

### Section 5: My Role and Scope

**Editorial headline:** Describe the scope principle in a phrase — not "My Role and Scope." Examples: "Product definition through experience design." / "End-to-end product lead, pilot through stable handoff." / "Defined scope and experience across nine stakeholder groups."

**Format:**
- Opening paragraph (75-100 words): Precise ownership. What did you personally own vs. what did engineering, design, data science, or your manager own? Name your cross-functional partners. Be explicit — a hiring manager reads this section for ownership clarity, not modesty.
- Then: Bulleted list of 5-8 specific areas you owned. Write as action phrases starting with verbs. Use only language consistent with source data — first-person ownership only where it was explicitly stated.

**Length:** 150-200 words.

---

### Section 6: Strategy and Decision-Making

**Editorial headline:** Name the central strategic question or the organizing principle for the decisions made. Examples: "Three decisions that shaped the product." / "Why biomarker extraction first." / "The central question: when should AI act automatically?"

**Format — choose based on source data:**

**If 2-4 named decisions are available:**
- Opening sentence: State the central strategic question or tension.
- For each decision, format as:

**[Decision name — short, bold, specific to this project]**
[2-4 sentences: what the options were, what was chosen, what tradeoff was accepted, and why. The reasoning chain must be explicit — do not just state the choice, explain the logic.]

**If 4+ discrete decisions with clear alternatives are available:**
- Opening sentence: State the central strategic question.
- Build a markdown decision table:

| Decision | Choice | Rationale |
|---|---|---|
| [Area] | [What was chosen] | [1-2 sentence rationale] |

- Follow the table with 1-2 paragraphs on the 1-2 most important decisions — the ones that required holding a position or accepting a significant tradeoff. Explain the human side: what the pressure was, how you held the line, what you accepted.

**Always include:** At least one decision where alternatives were explicitly considered and rejected. A strategy section with only the chosen direction demonstrates no judgment.

**Length:** 300-450 words.

---

### Section 7: Solution

**Editorial headline:** A phrase that names the design principle — not "Solution" or "What We Built." Examples: "A confidence-aware extraction pipeline." / "UX decisions from direct observation." / "One platform, two tiers, clear scheduling logic."

**Format:**
- Opening 1-2 sentences: Name the structural principle that organizes the solution (by user type, by feature, by pipeline stage, by component). State the organizing principle before naming the elements.
- Then: Named solution elements. Choose the breakdown principle based on what is most natural for this project:
  - **By user type:** Use when different users have materially different experiences. Label each block by user type. Explain what that user type gets and why it was designed that way.
  - **By feature or UX decision:** Use when the most important choices were interface or workflow decisions. Name each feature. Explain why it was designed the way it was — connect to the problem, insight, or constraint it addresses.
  - **By component or pipeline stage:** Use when the solution is a system with distinct technical or process stages. Name each stage. Explain its purpose and the design rationale.

Format each named element as:
**[Element name — bold noun phrase]**
[2-4 sentences: what it is, why it was designed this way, which problem, insight, or constraint it addresses. Do not just describe the feature — explain the reasoning behind it.]

**Pull quote:** If the source data contains a memorable first-person observation about a design decision or constraint, render it as an indented block.

> "[verbatim statement from source]"

**Length:** 300-400 words.

---

### Section 8: Execution and Iteration

**Editorial headline:** A phrase naming the scope constraint or execution principle. Examples: "Scope discipline enabled a five-month MVP." / "What the field taught us that requirements didn't."

**Format:**
- What bounded the MVP — which constraints shaped the scope, and which decisions were explicitly deferred and why.
- What changed mid-flight — 1-2 things that were different from the plan. What broke or surprised you and how it was handled.
- Where iteration focused — not a list of sprints, but the 1-2 things that kept evolving and the reasoning behind each iteration.

**Length:** 100-200 words.

---

### Section 9: Outcomes

**Editorial headline:** "Outcomes" with a subhead that frames the result type. Examples: "What shipped and what happened." / "Measurable workflow impact."

**Format:**
- Opening 1-2 sentences: What shipped and the overall result framing.
- Stat blocks: List 4-7 key metrics. Present in this order: (1) primary business or user metric, (2) quality or experience metric, (3) operational metric, (4) secondary business metrics. Use bold for the value, regular text for the label. If using directional indicators (up arrow / down arrow) for metrics without hard numbers, mark them clearly as directional, not absolute.
- Interpretation paragraph (75-125 words): Draw a conclusion from the data together. What does the combination of metrics signal? What is the clearest proof that the experience worked — not just that the feature shipped? Name the metric that tells the most interesting story.
- Unexpected outcome paragraph (if applicable): "One outcome was not anticipated..." Add this only if source data documents a specific result that was not the goal of the project or that contradicted an assumption.

**Length:** 200-300 words.

---

### Section 10: Reflection

**Editorial headline:** A phrase naming the core learning or the tension at the heart of the project. Examples: "Designing the boundary between AI and clinician." / "What I learned and would do differently." / "The pilot as the most valuable thing this project did."

**Format — choose based on depth and specificity of available reflection content:**

**Option A: Pithy-statement-led paragraphs (use when 3+ distinct learnings are available)**
Each paragraph opens with a short, declarative statement. The statement IS the lesson — the paragraph elaborates and grounds it in this specific project. The opening statement should be specific enough that it could not have come from any other project.

Format:
[Short declarative statement that is the lesson itself.] [2-4 sentences grounding it in a specific decision, moment, or data point from this project. Name the specific thing that taught this lesson.]

[Second lesson statement.] [Elaboration grounded in this project.]

[And so on for each distinct learning.]

**Option B: Two-part structure (use when 1-2 deep, specific learnings exist)**
- "The hardest part of this project was not [the obvious answer]. It was [the actual challenge]." [2-3 sentences expanding.]
- "If I ran this project again, I would [specific action]. [The specific decision I made] led to [specific consequence]. [What I'd have done instead]."
- "An open question remains: [genuine unresolved tension, framed as a real dilemma, not a rhetorical close]."

**Mandatory closing statement for both options:**
End with 1-2 sentences that generalize the core learning beyond this project. The closing statement should feel earned — a principle that holds beyond this case but was learned through it. It must be specific enough to signal domain expertise or genuine analytical depth. Generic closings fail: "communication is important" / "user research matters." Specific closings pass: "In clinical environments, AI adoption depends less on model accuracy and more on product decisions that make uncertainty visible and controllable."

**Rule:** Every named learning must be grounded in a specific decision, data point, or moment from this project. A learning that reads as a universal PM lesson without project-specific grounding must be rewritten or cut.

**Length:** 200-350 words.

---

## Writing Constraints (apply to both versions)

- No em dashes in generated prose. If source data contains a verbatim quote with an em dash, format it as a pull quote block — not woven into prose sentences.
- No bold markdown in prose paragraphs. Bold is allowed only for: numbered item labels in structured blocks, decision table headers.
- No invented metrics, outcomes, quotes, or ownership language. Every specific claim must trace to the source data.
- Use "I led" / "I owned" only when source data explicitly states first-person ownership. Default to "I contributed to" / "I partnered with" / "I supported."
- If launch status is pre-launch or pilot, state it explicitly in Context and Outcomes. Do not imply post-launch success.
- Safe phrasing when evidence is limited: "Early validation suggested..." / "Initial signals indicated..." / "The work established confidence in..." Do not use invented metrics to fill empty fields.
- No placeholder text in final output. If a section has no available evidence, shorten it or omit it — never fill with generics.

---

## Post-Generation Assets

After both versions, provide:

**Portfolio Headline Options (3):**
- Outcome angle: lead with the strongest metric or result
- Problem angle: name the problem and who experienced it
- PM skill angle: name the most distinctive judgment call demonstrated

**Subtitle Options (2):**
- Punchy: under 12 words, implies the PM's role or skill
- Descriptive: 15-25 words, names the problem and result

---

## Fallback: Single-Shot Prompt (For Thin Inputs)

Use this when the schema is very sparse and the full prompt would produce mostly empty sections:

```
You are a world-class PM portfolio case study writer.

Convert the project data below into two polished portfolio case studies:
1. A recruiter TLDR (350-500 words, five labeled sub-blocks: At a glance / The Problem / Why This Matters / What I Owned / Key Results)
2. A detailed hiring manager version (1,800-2,800 words, ten sections: Title and Thesis / Context / Problem / Users and Insights / My Role and Scope / Strategy and Decision-Making / Solution / Execution / Outcomes / Reflection)

Rules:
- Never invent facts, numbers, quotes, or outcomes.
- Never exaggerate ownership.
- If metrics are missing, emphasize validated signals, delivered scope, and learnings.
- If the project did not launch, make that explicit.
- Make the PM's thinking clear: insight to decision to action to outcome.
- No em dashes. Bold only for numbered item labels and table headers, never in prose.

For the TLDR: five labeled sub-blocks (At a glance / The Problem / Why This Matters / What I Owned as numbered list / Key Results as bullets).

For the detailed version: each section has an editorial headline (an assertion, not a label). The Problem section uses numbered bold-header dimensions. Users and Insights headline is the key finding. Strategy uses named decisions or a decision table. Solution is organized by a named structural principle (by user, by feature, or by component). Outcomes includes an interpretation paragraph. Reflection uses named units and ends with a generalizing closing statement.

Project data:
{{PROJECT_DATA}}
```

---

Structured project data:
{{PROJECT_SCHEMA_JSON}}

Writing preferences:
{{WRITING_PREFERENCES_JSON}}

Evidence check summary (if available):
{{EVIDENCE_CHECK_JSON}}
