---
name: build-portfolio
description: Assembles a complete, paste-ready vibe-coding prompt for a PM portfolio website from all generated case studies. Use when the user says "build my portfolio", "create my portfolio site", "turn my case studies into a website", "make a portfolio website", "I'm ready to publish", "I want to share my work", "ready to go live", "package my case studies into a site", or asks how to get a shareable portfolio. Auto-fires when the user finishes generating case studies and signals readiness to ship a site. Do NOT use for generating or writing case studies → use /generate instead. Do NOT use to start a new case study from scratch → use /quick-start instead.
---

# /build-portfolio

You are assembling a complete vibe-coding prompt for a product manager's portfolio. This skill collects inputs, reads all generated case studies, and produces a single structured prompt the user can paste directly into Lovable, Bolt, or v0 to build their portfolio website.

---

## Step 0 — Read Before You Write

| Source | Path | What to extract |
|--------|------|-----------------|
| Case study files | `outputs/*.md` (latest version per project, skip README.md) | Project title, company, timeline, role, 3-5 metrics from outcomes section |
| Portfolio inputs | `context-library/portfolio-inputs.md` | All personal details — name, UVP, career highlights, skills, target role |
| Writing preferences | `context-library/writing-preferences.md` | Tone, targeting, emphasis (load if exists and filled) |
| Generation prompt | `prompts/portfolio-generation-prompt.md` | Full assembler instructions for the vibe-coding prompt structure |

Do not generate output until Step 0 is complete.

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

After collecting answers, write all provided information to `context-library/portfolio-inputs.md` using the template structure in that file. For fields the user left blank: omit them from the saved file entirely — do not write empty placeholder text.

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

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "The user gave me their details inline — I can skip Step 4's save to portfolio-inputs.md" | Step 4 creates the persistent record needed to regenerate the prompt without re-interviewing. Skip it and the next run starts from scratch. |
| "The generation prompt is long — I can summarize it instead of executing it fully" | portfolio-generation-prompt.md contains the exact structure the vibe-coding tool expects. Summarizing it produces a prompt that breaks Lovable/Bolt/v0's parsing. Execute it in full. |
| "I can tell the prompt looks complete — I'll skip the placeholder scan before saving" | Placeholder text looks like content until it's pasted into Lovable and the site renders "[Your Name]" as a heading. Scan explicitly before saving. |
| "The user left most questions blank — I should ask follow-up questions to fill gaps" | Step 3 says answer what you can, skip anything that doesn't apply. Omit blank fields from portfolio-inputs.md. Do not re-ask. |
| "I can give generic filenames in the attachment instructions" | Step 7's attachment list must use the actual filenames from outputs/. Generic examples mislead the user into attaching the wrong files. |

## Before Marking Complete

Run this checklist before saving the prompt:

- [ ] Step 0 files were read — list which case study files and which prompts file were loaded
- [ ] Step 1 confirmed case study files exist — named them explicitly
- [ ] Step 2 determined the correct output filename (versioned if today's file already exists)
- [ ] Step 3 asked all 15 questions in one grouped message (not sequentially)
- [ ] Step 4 saved to context-library/portfolio-inputs.md — blank fields omitted, not left as placeholders
- [ ] Step 5 read each case study for metadata (title, company, timeline, role, 3-5 metrics)
- [ ] Step 6 executed portfolio-generation-prompt.md in full — design system, personal info, hero stats, case study cards, skills, about, contact all populated
- [ ] Placeholder scan: search the assembled prompt for any [bracket text] — if found, either populate from user input or remove the field entirely. Do not save with unfilled placeholders.
- [ ] Step 7 gives attachment instructions with actual filenames from outputs/ (not generic examples)

Only after all boxes checked: save and confirm to user.

## Step 7 — Save Output and Give Attachment Instructions

Save the assembled prompt to the filename determined in Step 2.

Confirm to the user with these exact instructions:

"Your portfolio prompt has been saved to [filename].

**To build your portfolio in Lovable, Bolt, or v0:**

1. Open `[filename]` and copy its full contents
2. Go to [Lovable](https://lovable.dev), [Bolt](https://bolt.new), or [v0](https://v0.dev) and start a new project
3. Attach these case study files to the chat (use the paperclip / file attachment button):
[List each actual case study filename with its project title — use real filenames from outputs/, e.g.:]
   - `[project-slug]-v1-[YYYY-MM-DD].md` ([Project Title])
   - `[project-slug]-v2-[YYYY-MM-DD].md` ([Project Title])
4. Paste the prompt text into the chat input alongside the attachments and send

The tool will generate a multi-page portfolio: a homepage (`index.html`) with case study cards, and one dedicated page per case study (`[project-slug].html`). Each card on the homepage links to its case study page. The prompt tells the tool exactly which section of each file to use for each UI component — you do not need to explain anything further.

**Once your site is live, your next step:** publish it to a public URL, then run `/review-portfolio` and paste the URL. I'll open your live site in a real browser, critique it the way a recruiter and hiring manager would for your target role, and hand you back prioritized fixes you can paste straight back into Lovable, Bolt, or v0. Building the site is the milestone; making sure it actually lands for recruiters is the finish line."

After saving and confirming, surface the `/review-portfolio` next step explicitly rather than ending silently — building the prompt is not the end of the journey; reviewing the deployed site is.

## Cross-Skill Routing

- If no case studies found in outputs/ → stop, say "No case studies found yet" and suggest running `/generate` or `/quick-start` first
- If user mentions wanting to add a new case study before building the portfolio → suggest `/quick-start` or `/generate`
- If user wants to refine an existing case study before publishing → suggest `/polish`
- After the user deploys the generated site to a public URL → suggest `/review-portfolio` to critique the live site and get paste-back fixes
