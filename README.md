# PM Portfolio Builder

Your work is strong. Your case studies should prove it and your portfolio should show it.

PM Portfolio Builder turns messy project material, including PRDs, notes, research, metrics, and the story in your head, into portfolio-ready product management case studies that hold up under recruiter and hiring manager review.

Once you have case studies, it can assemble them into a ready-to-paste prompt for Lovable, Bolt, or v0 so you can build a portfolio website from your strongest work.

It gives you a structured starting point instead of a blank page.

## What you get

For every case study, PM Portfolio Builder generates:

* A recruiter TLDR that is easy to scan
* A detailed hiring manager version with context, decisions, tradeoffs, outcomes, and reflection
* Visual placeholders and caption suggestions
* A polished final case study saved to your local file system

For your portfolio, it generates:

* A complete portfolio website prompt for a vibe coding tool of your choice
* Your positioning, case studies, skills, about section, and contact content
* A structured design direction for the site
* A versioned output file that does not overwrite previous work

After your portfolio is live, it can also review the site and return practical fixes for:

* Positioning
* Case study clarity
* Recruiter scanability
* Navigation
* Mobile experience
* Trust signals
* Visual polish

## When to use it

Use PM Portfolio Builder when you want to:

* Turn a product / project into a portfolio case study
* Convert scattered project docs into a clear PM story
* Show decisions, tradeoffs, outcomes, and scope
* Build a portfolio website from your resume and case studies
* Review a live portfolio before sharing it with recruiters or hiring managers

It works best when you have some evidence from the project, such as PRDs, strategy docs, launch notes, research summaries, metrics, stakeholder notes, screenshots, or resume bullets.

If you do not have documents, you can still use the interview flow. The system will ask structured questions and build the case study from your answers.

## Why it is different

### 1. It is evidence first

PM Portfolio Builder does not invent metrics, outcomes, quotes, launch results, or ownership claims.

### 2. It focuses on PM judgment

The output is structured to show how you think as a PM.

It highlights:

* Problem framing
* User and business context
* Decision-making
* Tradeoffs
* Stakeholder alignment
* Scope clarity
* Evidence
* Outcomes
* Reflection

### 3. It creates two versions

Recruiters need a quick scan. Hiring managers need depth. PM Portfolio Builder creates both. You choose the one you prefer.

### 4. It checks for overclaiming

Includes an evidence check before generation.

### 5. It improves with your feedback

Running `/learn` turns repeated feedback into reusable writing preferences, so future case studies need less rework.

### 6. It goes beyond the case study

PM Portfolio Builder takes you from raw project material to:

* Case study
* Portfolio website prompt
* Live portfolio review with correction prompts to paste into the site builder

## How it works

Start Claude Code in this folder and run:

```
/quick-start
```

Paste your project documents when prompted.

If you do not have documents, run:

```
/interview
```

The tool will ask guided questions and build the case study from your answers.

Once you have at least one case study, run:

```
/build-portfolio
```

This creates a portfolio website prompt you can paste into Lovable, Bolt, or v0.

After your site is live, run:

```
/review-portfolio
```

This reviews the live URL and returns prioritized fixes.

## What's Inside

```
pm-case-study-builder/
├── CLAUDE.md                        # System prompt — Claude reads this automatically
├── START-HERE.md                    # Read this first
├── .mcp.json                        # Bundled Playwright MCP for /review-portfolio (approve once)
├── setup/                           # Installation guide, checklist, and blank templates
│   ├── installation-guide.md        # How to install Claude Code
│   ├── first-session-checklist.md   # Step-by-step for your first session
│   ├── writing-preferences-template.md   # Blank template (auto-copied on first session)
│   ├── experience-library-template.md    # Blank template (auto-copied on first session)
│   └── example-writing-preferences.md   # Filled fictional example for reference
├── .claude/skills/                  # 13 skills that power the pipeline
│   ├── quick-start/                 # Full pipeline in one command
│   ├── interview/                   # Build a case study from a guided Q&A (no docs needed)
│   ├── ingest/                      # Document parsing and chunking
│   ├── extract/                     # Field-level structured extraction
│   ├── gap-detect/                  # Gap identification + targeted questions
│   ├── generate/                    # Case study generation (TLDR + detailed)
│   ├── visual-layer/                # Visual placeholders, plan, and caption suggestions
│   ├── polish/                      # Quality rewrite pass
│   ├── evidence-check/              # Pre-generation evidence validation
│   ├── tracker/                     # Portfolio coverage tracker
│   ├── build-portfolio/             # Assemble full portfolio website prompt
│   ├── review-portfolio/            # Critique the LIVE portfolio site (Playwright MCP)
│   └── learn/                       # Style synthesis from editing behavior
├── context-library/                 # Your data — auto-created, never committed to git
│   ├── project-inputs-[slug].md     # Per-project structured data (one file per case study)
│   ├── writing-preferences.md       # Tone, targeting, emphasis (filled on first session)
│   ├── experience-library.md        # Cross-project experience bank (optional)
│   └── portfolio-inputs.md          # Resume, LinkedIn, personal details for portfolio
├── sub-agents/                      # Reviewer agents
│   └── recruiter-reviewer.md        # Recruiter-lens critique sub-agent
├── prompts/                         # Raw prompt library (the engine)
│   ├── system-prompt.md             # Core generator system prompt
│   ├── extraction-prompts.md        # Per-field extraction prompts
│   ├── gap-detection-prompt.md      # Gap detection prompt
│   ├── generation-prompt.md         # Main generation prompt
│   ├── visual-planning-prompt.md    # Visual plan, placeholder, and caption prompts
│   ├── evidence-check-prompt.md     # Evidence validation prompt
│   └── polish-prompt.md             # Quality rewrite prompt
├── templates/                       # Output templates
│   ├── project-schema.json          # Structured data schema
│   ├── recruiter-tldr.md            # TLDR output template
│   └── hiring-manager-detailed.md   # Detailed version template
├── insider-data/                    # PM evaluation frameworks
│   └── pm-frameworks/               # What recruiters and hiring managers look for (not committed to git)
├── outputs/                         # Generated case studies (not committed to git)
│   └── portfolio/                   # Generated portfolio prompt (portfolio-prompt-[date].md)
└── briefings/                       # Feedback log and session notes (not committed to git)
    └── feedback-log.md              # Auto-created on first session; input to /learn
```

## Outputs

Generated case studies are saved in:

```text
outputs/[project-slug]-v[N]-[YYYY-MM-DD].md
```

Portfolio prompts are saved in:

```text
outputs/portfolio/portfolio-prompt-[YYYY-MM-DD].md
```

Portfolio reviews are saved in:

```text
outputs/portfolio-reviews/[YYYY-MM-DD].md
```

Files are versioned so previous outputs are not overwritten.

## Supported Input Formats

* Paste raw text directly (PRDs, notes, bullets, research, anything)
* Reference file paths for PDFs, DOCX, TXT files in the project folder

## Recommended first run

1. Open this folder in Claude Code.
2. Run:

```bash
/quick-start
```

3. Paste one project worth of documents or notes.
4. Answer any follow-up questions.
5. Review the case study in `outputs/`.
6. Repeat for your strongest projects.
7. Run:

```bash
/build-portfolio
```

8. Paste the generated portfolio prompt into Lovable, Bolt, or v0.
9. Publish the site.
10. Run:

```bash
/review-portfolio
```

## Prerequisites

You will need:

* A Claude subscription that supports Claude Code
* A terminal or code editor such as Cursor or VS Code

See `setup/installation-guide.md` for setup instructions.

## Created by

Mrunal Surve
AI Product Manager
Portfolio: https://www.mrunalsurve.com/
LinkedIn: https://www.linkedin.com/in/mrunal-surve-iimi/

I built PM Portfolio Builder after seeing the same problem repeatedly: product managers often do meaningful work, but their portfolios do not always show the quality of their decisions.

This project helps turn real project evidence into clearer case studies and portfolio content.
