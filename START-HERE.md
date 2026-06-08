# Start Here — PM Portfolio Builder

## What You Need

1. **A Claude subscription.** Claude Pro ($20/month) or Max ($100/month) at [claude.ai](https://claude.ai). This gives you access to Claude Code.
2. **Claude Code installed.** Follow `setup/installation-guide.md` (5 minutes). Once installed, type `claude` in a terminal and start talking.
3. **A code editor or terminal.** [Cursor](https://cursor.com) is recommended (free, built-in terminal). VS Code also works.
4. **Your project documents.** Any combination of PRDs, research notes, metrics reports, strategy docs, resume bullets, or raw notes for one project.

**Time to first case study:** About 20-30 minutes. After that, each new case study takes 15-20 minutes.
**Time to full portfolio:** About 10-15 additional minutes after your case studies are done.

---

## Try It Now (2 min)

Open this folder in your terminal, start Claude Code with `claude`, and run:

```
/quick-start
```

Then paste your project documents. You'll have a draft case study before you finish your coffee. No documents written down? Say `interview me` instead and answer a guided set of questions — same result, no docs required.

---

## Full Setup (20 min)

**Step 1: Writing preferences (automatic on first session)**
When you open Claude Code for the first time, it will ask you 5 questions about your role, target companies, and PM style. Your answers are saved to `context-library/writing-preferences.md` and applied to every case study automatically. You can also say `set up my writing preferences` to re-run this at any time, or paste your resume and say "fill my writing preferences from this." See `setup/example-writing-preferences.md` for a filled example.

**Step 2: Build your experience library (optional, 10 min)**
Say `build my experience library from this` and paste your resume and LinkedIn. Claude organizes your story bank so future case studies draw from the same evidence pool. See `setup/writing-preferences-template.md` for the structure.

**Step 3: Run your first case study (10-15 min)**
Say `build my case study` and paste your documents. Or run `/quick-start` for a fast first draft. If the project is only in your head with nothing written down, say `interview me` and Claude will ask you ~15 targeted questions, then run the same pipeline.

**Step 4: Build your portfolio (10-15 min)**
After generating your case studies, say `build my portfolio` or run `/build-portfolio`. Paste your resume and answer a few questions about your career positioning, skills, and target role. You'll get a structured vibe-coding prompt saved to `outputs/portfolio/portfolio-prompt-[YYYY-MM-DD].md`. Copy the full contents and paste it into Lovable, Bolt, or v0 — the prompt is self-contained and the tool will build and style your site without any additional context from you.

---

## Your First 3 Commands

Run these in order after setup:

1. `/quick-start [paste your project docs]` — First draft in one pass. Zero setup.
2. `Help me fill out my writing preferences` — Sets tone, targeting, and emphasis for all future case studies.
3. `/generate` — Full pipeline with polished output, saved to `outputs/`.

---

## What You Get

**For every case study:**
- Recruiter TLDR (350-500 words) — scannable, front-loaded with signal
- Detailed hiring manager version (1,200-2,000 words) — causal narrative with decisions, tradeoffs, reflection
- 3 portfolio headline options
- 2 subtitle options
- Visual caption suggestions (if you have screenshots or diagrams)

**For your portfolio:**
- Structured vibe-coding prompt saved to `outputs/portfolio/portfolio-prompt-[YYYY-MM-DD].md`
- Self-contained — paste the full file into Lovable, Bolt, or v0 with no additional context needed
- Includes your design system, all personal content, and all case study content in full
- The vibe-coding tool generates a mobile-responsive portfolio site with hero, case studies, skills, about, and contact
- Versioned — each run creates a new file, never overwrites prior output

**Built-in reviewers:**
- `/review-as-recruiter` — Would they pass this to a hiring manager?
- `/review-as-hiring-manager` — Does this show real PM judgment?

---

## Available Commands

Start with the three commands above. For the complete command reference — every pipeline step plus the recruiter and hiring-manager reviews, tracker, and learn — see the Available Commands table in [`CLAUDE.md`](CLAUDE.md), the single source of truth for commands.

---

## What to Fill In

Claude auto-creates these files on your first session and walks you through setup:
- `context-library/writing-preferences.md` — filled via 5-question onboarding on first session
- `context-library/experience-library.md` — say "build my experience library from this" and paste your resume

After your first case study:
- `context-library/project-inputs-[slug].md` — created automatically by `/quick-start` or `/ingest`

See `setup/first-session-checklist.md` to get oriented fast.

---

## Guardrails (Built In)

This tool never invents metrics or outcomes. It never overstates your role. It handles pre-launch, pilot, and no-metric projects gracefully. It asks max 5 questions, never more.

---

Need help with installation? See `setup/installation-guide.md`.
