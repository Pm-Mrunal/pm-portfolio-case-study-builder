# Portfolio Prompt Assembler

You are assembling a structured vibe-coding prompt that the user will paste into Lovable, Bolt, or v0 to generate their PM portfolio website. Your output is a markdown document — not HTML — written as a clear product brief for the AI in those tools.

The case study files are **not** embedded inline in this prompt. Instead, the user will attach those `.md` files directly to the vibe-coding tool alongside this prompt. This prompt tells the tool which files are attached, which section of each file maps to which UI component, and how to use them.

Your job in this step is:
1. Fill the prompt's design system, personal info, hero stats, AI strip, skills, about, and contact sections with real content from `portfolio-inputs.md` and the resume.
2. For each case study, extract only the metadata needed for the card and sidebar: title, anchor ID, tags, sidebar metrics (the specific numbers), and a 1-2 sentence problem statement. Do not rewrite or summarize the case study narrative — the attached files handle that.
3. Write the file reference instructions that tell the vibe-coding tool exactly how to use each attached case study file.

---

## Hard Rules

- Never invent facts, metrics, user quotes, outcomes, or role ownership.
- Every metric cited in the sidebar or hero stats must appear verbatim in the source files.
- Do not include placeholders like "[insert here]" in the final output.
- Do not mention AI, Claude, or any generation tool in the prompt text.
- Do not use em dashes in body text sections.
- If a field in portfolio-inputs.md is blank, omit that element gracefully.
- Use safe phrasing for missing evidence: "early signals suggested", "contributed to", "initial validation showed".

---

## Output Format

Write the assembled prompt as a single markdown document using the section structure below. Write the prompt in second person from the perspective of a product brief given to a developer ("Build a portfolio website for..."), not from Claude's perspective.

---

## Template to Fill and Output

```
# PM Portfolio Website — Build Brief

## What You're Building

Build a complete, single-page PM portfolio website for [NAME], a [CURRENT ROLE] based in [LOCATION]. The site showcases [N] case studies, a skills section, an about section, and contact links. It should feel like a product-builder portfolio, not a generic PM CV site.

Target audience: hiring managers and recruiters at [TARGET INDUSTRY] companies looking for [TARGET ROLE] candidates. The design and copy should signal credibility, specificity, and technical depth.

---

## Attached Case Study Files

I am attaching [N] case study files alongside this prompt. Each file has two distinct sections:

- `=== RECRUITER TLDR ===` — a concise version (300-500 words) with the problem statement, key decisions, and top outcome. Use this to populate the **case study card** in the grid.
- `=== DETAILED HIRING MANAGER VERSION ===` — the full narrative (1,500-3,000 words) with problem, discovery, strategy, execution, impact, and learnings. Use this to populate the **full case study detail section** below the card grid.

Do not summarize or paraphrase these sections. Render the content as written, formatted into the appropriate UI components.

The attached files are:
[For each case study, list the filename and its anchor ID:]
- `[filename.md]` → case study anchor ID: `#[project-slug]`
- `[filename.md]` → case study anchor ID: `#[project-slug]`
[...repeat for each case study]

---

## Design System

### Colors
[Fill with the user's specified color preference. Derive a 6-color palette with hex values and roles. If no preference was stated, use the default below:]

Default (blue/gray/white):
- Background: #FFFFFF
- Surface (cards, sections): #F9FAFB
- Border: #E5E7EB
- Primary accent: #1A56DB (blue)
- Primary text: #111827 (near-black)
- Secondary text: #6B7280 (gray)
- Dark section: #111827

Warm cream / charcoal / muted blue (use when user specifies this):
- Background: #FAF8F5
- Surface: #F0EDE8
- Border: #D9D4CC
- Primary accent: #3A6B8A (muted slate blue)
- Primary text: #2C2C2C (charcoal)
- Secondary text: #6B6B6B (medium gray)
- Dark section: #1A1A1A

### Typography
- Font stack: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif
- H1: 2.75rem, weight 800, letter-spacing -0.03em
- H2: 1.75rem, weight 700, letter-spacing -0.02em
- H3: 1.2rem, weight 700
- Body: 1rem / 1.65 line height
- Labels/eyebrows: 0.72rem, weight 700, uppercase, letter-spacing 0.1em
- Max content width: 1100px, centered

### Spacing
- Section padding: 88px top/bottom on desktop, 52px on mobile
- Card padding: 32px
- Card gap: 24px

### Style Rules
- Minimalist, professional, warm (not cold tech)
- No box shadows heavier than 0 2px 8px rgba(0,0,0,0.08)
- Subtle fade-in on scroll only — no flashy animations
- Borders and whitespace over decorative elements
- Mobile responsive at 768px breakpoint

---

## Page Structure

Build the following sections as a single-page layout with smooth scroll navigation:

1. **Sticky Navigation** — Name/logo left, nav links right (About, Case Studies, Skills, Contact). Mobile hamburger menu. Active link highlights on scroll.
2. **Hero** — Name, role, location, UVP, two CTA buttons, 4-stat metrics row.
3. **AI Work Strip** — Narrow horizontal band with 4 columns (only if AI work narrative applies — see content below).
4. **Case Study Cards** — 2-column grid. Each card links to its full detail section via smooth scroll.
5. **Case Study Detail Sections** — One full section per case study, with sidebar and body content drawn from the attached files.
6. **Skills Section** — 3 columns: Technical Skills, Product Skills, Domain Expertise, displayed as pill tags.
7. **About Section** — Bio paragraph left, career highlights and certifications right.
8. **Contact Section** — Dark background, centered, email + LinkedIn + portfolio site links.
9. **Footer** — Name, location, year.

UX requirements:
- Active nav link highlights on scroll using IntersectionObserver
- Case study card "Read More" buttons smooth-scroll to the corresponding detail section
- Back-to-top button visible after 400px scroll
- All external links open in new tab
- All interactive elements keyboard accessible via Tab
- Fade-in animation on cards and major sections as they enter the viewport

---

## Hero Content

**Name:** [NAME]
**Role:** [CURRENT ROLE]
**Location:** [LOCATION]
**UVP:** [UNIQUE VALUE PROPOSITION — paste verbatim from portfolio-inputs.md]

**CTA 1:** View Case Studies → scrolls to #case-studies
**CTA 2:** Get in Touch → mailto:[EMAIL]

**Hero Stats:**
Display as 4 large stat blocks in a row (2-column on mobile). Pull the 4 most impactful numbers from the Career Highlights and case study outcomes in portfolio-inputs.md. Format: large bold value + short descriptive label.

[Fill all 4 stats with real numbers from portfolio-inputs.md and the loaded case study metadata:]
- Stat 1: [value] / [label]
- Stat 2: [value] / [label]
- Stat 3: [value] / [label]
- Stat 4: [value] / [label]

---

## AI Work Strip

[Only include this section if the user has AI work across at least 3 distinct tracks. Omit entirely if not applicable.]

Display as a narrow horizontal band immediately below the hero, with 4 labeled columns:

- **[Column 1 label]:** [content — e.g. "Shipped at work" / "Document AI for oncology EMR · RAG assistant for lab operations"]
- **[Column 2 label]:** [content — e.g. "Self-built" / "OutFlow AI agent (30 active users) · DRAPE AI (2 SMB clients)"]
- **[Column 3 label]:** [content — e.g. "Architecture and evaluation" / "RAG · LLM-as-judge · Confidence routing · Eval strategy"]
- **[Column 4 label]:** [content — e.g. "Teaching and advising" / "AI Product Bootcamp · 30+ professionals trained"]

[Fill each column from the resume and portfolio-inputs.md only. Do not invent entries.]

---

## Case Study Cards

Display as a 2-column grid (1-column on mobile). Order: put the strongest AI or builder-signal project first.

[For each case study, fill one card block. Extract the problem statement and top outcome from the RECRUITER TLDR section of the corresponding attached file. Do not rewrite — pull verbatim or very close to verbatim.]

### Card [N]: [PROJECT TITLE]
**Anchor:** #[project-slug]
**Tags:** [2-4 short tags, e.g. "AI Product", "0-to-1", "Consumer", "B2G" — derived from project type]
**Problem (1-2 sentences):** [Pulled from the opening of the RECRUITER TLDR in the attached file — the "what was broken" framing]
**Top outcome (displayed as a highlighted callout with left border):** [The lead metric or result from the RECRUITER TLDR — the single most impressive number or outcome]
**Read More button:** smooth-scrolls to #[project-slug]

[Repeat for each case study]

---

## Case Study Detail Sections

One full-width section per case study. Display in the same order as the cards above.

Each section has a two-column layout on desktop:
- **Left column (sidebar, ~240px, sticky on desktop):** project metadata and sidebar metrics
- **Right column (body):** full case study narrative

### Detail: [PROJECT TITLE]
**Section ID:** [project-slug]

**Sidebar metadata:**
- Company: [company name — from portfolio-inputs.md or resume]
- Timeline: [duration and year — from portfolio-inputs.md or resume]
- Role: [PM role title — from portfolio-inputs.md or resume]
- Type: [product type — e.g. "Consumer Fintech / AI Agent"]

**Sidebar metrics:**
Display as 3-5 large stat blocks (bold number + short label). Pull these specific numbers from the Results/Impact section of the attached file:
[For each case study, extract the 3-5 most specific metrics from the Impact section of the attached file and list them here:]
- [value] / [label]
- [value] / [label]
- [value] / [label]
[...up to 5]

**Body content:**
Render the full `=== DETAILED HIRING MANAGER VERSION ===` section from the attached `[filename.md]` file. Structure the content into these labeled subsections, using the corresponding parts of the file:

- **Problem** — from the Problem section of the detailed version
- **Discovery** — from the Users and Insights or Discovery section
- **Strategy** — from the Strategy and Decision-Making section; use a styled callout block (left border, light background) for the most important decision or key insight
- **Execution** — from the Execution section
- **Impact** — from the Outcomes/Results section; display as a bulleted list
- **Key Learnings** — from the Reflection or Learnings section

Do not summarize or paraphrase. Render the content as written, broken into the subsections above.

[Repeat for each case study]

---

## Skills Content

Display as 3 columns (stack to 1 column on mobile). Each skill is a pill tag: small font, rounded border, light background.

### Technical Skills
[List each skill as a separate pill. Pull verbatim from the Technical Skills field in portfolio-inputs.md:]
[skill 1] · [skill 2] · [skill 3] · ...

### Product Skills
[List each skill as a separate pill. Pull verbatim from the Product Skills field in portfolio-inputs.md:]
[skill 1] · [skill 2] · [skill 3] · ...

### Domain Expertise
[List each item as a separate pill. Pull verbatim from the Domain Expertise field in portfolio-inputs.md:]
[item 1] · [item 2] · [item 3] · ...

---

## About Content

**Bio (4 paragraphs):**
[Write the bio from these sources only: current role, years of experience, UVP, target role, concerns to address, and LinkedIn summary from portfolio-inputs.md. Follow this structure — do not deviate:]
- Paragraph 1: What I do and how I work — builder orientation, end-to-end approach, breadth of markets
- Paragraph 2: Why the domain background is a strength, not a ceiling — addresses the concern about being seen as domain-specific
- Paragraph 3: What I specifically bring to an AI product or platform role — technical depth, evaluation mindset, shipping track record
- Paragraph 4: What I'm looking for — target role type, working style preference

**Career Highlights (right sidebar, bulleted list):**
[Pull verbatim from the Career Highlights field in portfolio-inputs.md. 6-8 bullets with measurable outcomes.]

**Education and Certifications (below highlights):**
[Pull from portfolio-inputs.md or resume. List degree, institution, year, then certifications.]

---

## Contact Content

**Section background:** dark (#1A1A1A or equivalent)
**Heading:** Let's Talk
**Subtext:** Open to [TARGET ROLE] opportunities. [LOCATION], open to remote and hybrid roles.

**Links (display as styled buttons):**
- Primary button: [EMAIL] — mailto link
- Secondary button: LinkedIn — [LINKEDIN URL], opens in new tab
- Secondary button: [PORTFOLIO SITE LABEL] — [PORTFOLIO URL], opens in new tab (only if URL was provided)

---

## Technical Requirements

- React component output preferred (for Lovable/v0); pure HTML also acceptable (for Bolt)
- No external CDN links or Google Fonts API calls — use the system font stack above
- No placeholder images or Lorem Ipsum text anywhere in the output
- Mobile responsive at 768px with a single breakpoint
- Semantic HTML5 structure with correct heading hierarchy (one H1, multiple H2s, H3s within sections)
- All interactive elements keyboard accessible
- Smooth scroll behavior for all anchor links
- IntersectionObserver for fade-in on cards and case study sections
- Back-to-top button appears after 400px scroll
```

---

## Assembly Checklist (run before saving the output)

1. Every metric in the hero stats and sidebar metrics appears verbatim in the source files.
2. No section contains placeholder text or unfilled brackets.
3. The case study order leads with the strongest AI or builder signal.
4. The AI Work Strip columns (if included) are grounded in resume and portfolio-inputs.md content only.
5. The attached file list in "Attached Case Study Files" matches the actual files that will be attached.
6. The bio paragraphs draw only from portfolio-inputs.md — no invented personal details.
7. Skills pills are drawn verbatim from portfolio-inputs.md — no additions.
