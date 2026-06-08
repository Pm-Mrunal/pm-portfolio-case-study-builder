# PM Portfolio Builder

Turn messy project documents into portfolio-ready PM case studies — then assemble everything into a structured vibe-coding prompt you can paste into Lovable, Bolt, or v0 to generate your portfolio website. All in one terminal session.

## How It Works

Open this folder in Claude Code. Paste your project documents — or, if the project is only in your head and you have nothing written down, say "interview me" and answer a guided series of questions instead. Either way, answer a few questions. Get a recruiter TLDR and a detailed hiring manager version, saved to your file system. Before generating, `/quick-start` runs a silent evidence check that warns you only if your outcomes look thin or your role ownership risks overclaiming — so the fast path stays fast but never quietly ships an overclaimed case study. When you have at least one case study, run `/build-portfolio` to generate a vibe-coding prompt you can paste into Lovable, Bolt, or v0.

```
/quick-start       →  paste docs, get case study
/interview         →  no docs? answer questions, get case study
/build-portfolio   →  assemble everything into a portfolio website
/review-portfolio  →  critique your live portfolio URL as a recruiter would
```

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
│   ├── visual-layer/                # Visual plan, placeholders, and captions
│   ├── polish/                      # Quality rewrite pass
│   ├── evidence-check/              # Pre-generation evidence validation
│   ├── tracker/                     # Portfolio coverage tracker
│   ├── build-portfolio/             # Assemble full portfolio website
│   ├── review-portfolio/            # Critique the LIVE portfolio site (Playwright MCP)
│   └── learn/                       # Style synthesis from editing behavior
├── context-library/                 # Your data — auto-created, never committed to git
│   ├── project-inputs-[slug].md     # Per-project structured data (one file per case study)
│   ├── writing-preferences.md       # Tone, targeting, emphasis (filled on first session)
│   ├── experience-library.md        # Cross-project experience bank (optional)
│   └── portfolio-inputs.md          # Resume, LinkedIn, personal details for portfolio
├── sub-agents/                      # Reviewer agents
│   ├── recruiter-reviewer.md        # Recruiter lens critique
│   └── hiring-manager-reviewer.md   # Hiring manager lens critique
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
│   └── pm-frameworks/               # What recruiters and HMs actually look for
├── outputs/                         # Generated case studies land here (not committed to git)
│   └── portfolio/                   # Generated portfolio prompt (portfolio-prompt-[date].md)
└── briefings/                       # Feedback log and session notes (not committed to git)
    └── feedback-log.md              # Auto-created on first session; input to /learn
```

## Outputs

All generated case studies save to `outputs/` with the filename:
`[project-slug]-v[N]-[YYYY-MM-DD].md`

The portfolio vibe-coding prompt saves to `outputs/portfolio/portfolio-prompt-[YYYY-MM-DD].md`. Subsequent runs on the same day version as `-v2`, `-v3`, etc. — prior prompts are never overwritten. Copy the full file contents and paste into Lovable, Bolt, or v0 to generate your portfolio site.

Once the site is live, `/review-portfolio` critiques the deployed URL and saves the scored review (with paste-back fixes and screenshots) to `outputs/portfolio-reviews/[YYYY-MM-DD].md`.

## Commands

The two commands you need to start:

| Command | What It Does |
|---|---|
| `/quick-start` | Full pipeline in one command — paste docs, get case study |
| `/build-portfolio` | Assemble your case studies into a paste-ready vibe-coding prompt for Lovable, Bolt, or v0 |

For the complete command reference (interview, ingest, extract, gap-detect, generate, visual-layer, polish, evidence-check, the recruiter and hiring-manager reviews, review-portfolio, tracker, and learn), see the Available Commands table in [`CLAUDE.md`](CLAUDE.md) — the single source of truth for commands.

## Supported Input Formats

- Paste raw text directly (PRDs, notes, bullets, research, anything)
- Reference file paths for PDFs, DOCX, TXT files in the project folder

---

## Created By

**Mrunal Surve | AI Product Manager**
🌐 [mrunalsurve.com](https://www.mrunalsurve.com/) | 💼 [LinkedIn](https://www.linkedin.com/in/mrunal-surve-iimi/)

Mrunal Surve is an AI Product Manager with 7+ years of experience building digital products across B2B, B2G, and B2C in Asia, Africa, and North America. He built PM Portfolio Builder as a solo project after seeing the same problem repeatedly — product managers doing genuinely strong work but unable to articulate it in writing that holds up under recruiter and hiring manager scrutiny. The tool encodes what good PM case studies actually look like, so the output isn't just polished — it reflects real judgment.

He also runs an AI Builder Bootcamp for product managers: a hands-on program covering strategy, agent design, and evaluation frameworks, helping PMs go from zero to shipping agentic AI products end-to-end.
