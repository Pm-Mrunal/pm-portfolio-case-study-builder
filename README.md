# PM Portfolio Builder

Turn messy project documents into portfolio-ready PM case studies — then assemble everything into a structured vibe-coding prompt you can paste into Lovable, Bolt, or v0 to generate your portfolio website. All in one terminal session.

## How It Works

Open this folder in Claude Code. Paste your project documents. Answer a few questions. Get a recruiter TLDR and a detailed hiring manager version, saved to your file system. When you have at least one case study, run `/build-portfolio` to generate a vibe-coding prompt you can paste into Lovable, Bolt, or v0.

```
/quick-start       →  paste docs, get case study
/build-portfolio   →  assemble everything into a portfolio website
```

## What's Inside

```
pm-case-study-builder/
├── CLAUDE.md                        # System prompt — Claude reads this automatically
├── START-HERE.md                    # Read this first
├── setup/                           # Installation guide + first session checklist
├── .claude/skills/                  # 9 skills that power the pipeline
│   ├── quick-start/                 # Full pipeline in one command
│   ├── ingest/                      # Document parsing and chunking
│   ├── extract/                     # Field-level structured extraction
│   ├── gap-detect/                  # Gap identification + targeted questions
│   ├── generate/                    # Case study generation (TLDR + detailed)
│   ├── visual-layer/                # Visual plan, placeholders, and captions
│   ├── polish/                      # Quality rewrite pass
│   ├── evidence-check/              # Pre-generation evidence validation
│   ├── tracker/                     # Portfolio coverage tracker
│   └── portfolio/                   # Assemble full portfolio website
├── context-library/                 # Your data (fill these in, not committed to git)
│   ├── project-inputs-[slug].md     # Per-project structured data (one file per case study)
│   ├── writing-preferences.md       # Tone, targeting, emphasis
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
│   └── portfolio/                   # Generated vibe-coding prompt (portfolio-prompt-[date].md)
└── briefings/                       # Session notes and summaries
```

## Outputs

All generated case studies save to `outputs/` with the filename:
`[project-slug]-v[N]-[YYYY-MM-DD].md`

The portfolio vibe-coding prompt saves to `outputs/portfolio/portfolio-prompt-[YYYY-MM-DD].md`. Subsequent runs on the same day version as `-v2`, `-v3`, etc. — prior prompts are never overwritten. Copy the full file contents and paste into Lovable, Bolt, or v0 to generate your portfolio site.

## Commands

| Command | What It Does |
|---|---|
| `/quick-start` | Full pipeline in one command — paste docs, get case study |
| `/ingest` | Parse and chunk project documents |
| `/extract` | Extract structured data field-by-field |
| `/gap-detect` | Find what's missing, ask 3-5 targeted questions |
| `/generate` | Generate recruiter TLDR + detailed case study |
| `/visual-layer` | Add visual plan, placeholders, and captions |
| `/polish` | Quality rewrite pass |
| `/evidence-check` | Validate evidence before generating |
| `/tracker` | Track portfolio coverage |
| `/build-portfolio` | Collect resume + LinkedIn + personal details → generate vibe-coding prompt for Lovable, Bolt, or v0 |

## Supported Input Formats

- Paste raw text directly (PRDs, notes, bullets, research, anything)
- Reference file paths for PDFs, DOCX, TXT files in the project folder
