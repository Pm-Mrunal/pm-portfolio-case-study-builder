---
name: build-portfolio
description: Collect resume + LinkedIn + personal details → generate a ready-to-paste vibe-coding prompt for Lovable, Bolt, or v0 that builds the full portfolio website.
---

# /build-portfolio

You are assembling a complete vibe-coding prompt for a product manager's portfolio. This skill collects inputs, reads all generated case studies, and produces a single structured prompt the user can paste directly into Lovable, Bolt, or v0 to build their portfolio website.

---

## Step 1 — Check for Generated Case Studies

Read the `outputs/` directory and list all `.md` files (excluding `README.md`). These are the case studies that will populate the portfolio.

If no `.md` case study files exist in `outputs/`, stop and say:
"No case studies found yet. Run `/quick-start` and generate at least one case study before building your portfolio prompt."

If case studies exist, list them by filename and continue.

---

## Step 2 — Check for Existing Prompt Files

Check whether any files matching `outputs/portfolio/portfolio-prompt-*.md` exist.

If any exist, note this to the user: "An existing portfolio prompt was found. A new file will be created — your prior prompt will not be overwritten."

Determine the output filename for this run:
- Check today's date (use the `currentDate` from context if available, otherwise use today's date).
- If no file named `outputs/portfolio/portfolio-prompt-[YYYY-MM-DD].md` exists, use that name.
- If it already exists, check for `-v2`, `-v3` etc. and use the next available version.

---

## Step 3 — Collect Portfolio Inputs

Tell the user: "I found [N] case studies. Now I need a few details to build your portfolio prompt. Answer what you can — skip anything that doesn't apply."

Ask the following questions in a single grouped message (do not ask one at a time):

1. **Name** — Your full name as it should appear on the portfolio
2. **Current role** — Title and company (e.g., "Senior Product Manager at Acme Health")
3. **Location** — City and country (e.g., "San Francisco, CA")
4. **Years of experience** — Total years in product management or related roles
5. **Target role** — The role type you are targeting (e.g., "Senior PM", "AI Product Manager", "Platform PM")
6. **Target industry or companies** — Where you want to land (e.g., "AI/ML SaaS", "Series B healthtech", "FAANG")
7. **Unique Value Proposition** — One sentence that captures what makes you distinct as a PM
8. **Career highlights** — 5–8 bullet points with measurable outcomes (copy from resume if easier)
9. **Technical skills** — Tools, platforms, languages, analytics tools you use
10. **Product skills** — e.g., roadmapping, PRD writing, user research, experimentation
11. **Domain expertise** — Industry knowledge or product domains you know deeply
12. **Color preference** — Default is blue/gray/white. Mention if you prefer another scheme.
13. **Concerns to address** — Career gap, industry transition, limited PM years, anything to handle proactively (optional)
14. **Resume** — Paste your resume text, or provide a file path to a PDF/TXT in this folder (optional but recommended)
15. **LinkedIn summary** — Paste your LinkedIn About section or headline (optional)

Wait for the user's response before proceeding.

---

## Step 4 — Save Inputs to portfolio-inputs.md

After collecting answers, write all provided information to `context-library/portfolio-inputs.md` using the template structure in that file. Do not overwrite fields the user left blank — leave them as empty placeholders.

Confirm: "Inputs saved to context-library/portfolio-inputs.md."

---

## Step 5 — Read Source Files (Metadata Only for Case Studies)

Read the following files in full:
- `context-library/portfolio-inputs.md`
- `context-library/writing-preferences.md` (if it exists and is filled)

For each case study `.md` file found in `outputs/` (use the latest version of each project, skip README.md):
- Read the file
- Extract only: project title, company, timeline, role, product type, and the 3-5 specific metrics from the Impact/Results section (for the sidebar)
- Do NOT rewrite or summarize the case study narrative — that content will be attached directly to the vibe-coding tool as a file

Record the exact filename for each case study (the user will need to attach these files).

Say: "Loaded [N] case studies + portfolio inputs. Assembling your vibe-coding prompt..."

---

## Step 6 — Assemble the Vibe-Coding Prompt

Execute the assembler instructions in `prompts/portfolio-generation-prompt.md`.

Use portfolio-inputs.md and the extracted case study metadata as inputs. Fill the design system, personal info, hero stats, AI strip, card summaries, sidebar metrics, skills, about, and contact sections with real content. Do not invent any facts, metrics, or outcomes not present in the source files.

For each case study detail section: write the file reference instruction (which attached file to use, which section maps to the card, which maps to the full detail) — do not embed the narrative.

The output of this step is a structured markdown document — not HTML — that the user will paste into Lovable, Bolt, or v0.

---

## Step 7 — Save Output and Give Attachment Instructions

Save the assembled prompt to the filename determined in Step 2.

Confirm to the user with these exact instructions:

"Your portfolio prompt has been saved to [filename].

**To build your portfolio in Lovable, Bolt, or v0:**

1. Open `[filename]` and copy its full contents
2. Go to [Lovable](https://lovable.dev), [Bolt](https://bolt.new), or [v0](https://v0.dev) and start a new project
3. Attach these case study files to the chat (use the paperclip / file attachment button):
[List each case study filename with its project title, e.g.:]
   - `outflow-ai-v2-2026-04-16.md` (OutFlow AI)
   - `document-ai-oncology-v1-2026-03-31.md` (Document AI for Oncology)
   - `myvisit-homecare-consolidation-v3-2026-03-31.md` (MyVisit and HomeCare)
   - `health-campaign-management-v4-2026-04-01.md` (Health Campaign Management)
4. Paste the prompt text into the chat input alongside the attachments and send

The prompt tells the tool exactly which section of each file to use for each UI component — you do not need to explain anything further. The tool will generate the full portfolio site from there."
