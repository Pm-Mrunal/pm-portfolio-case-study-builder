---
name: review-portfolio
description: Critiques a user's LIVE, deployed portfolio website by visiting the URL in a real browser (Playwright MCP) and reviewing it as a recruiter and hiring manager would. Captures desktop and mobile screenshots, the accessibility tree, and contrast/load data, then scores the site on two axes — portfolio purpose (does it win an interview for the target role) and UI/UX craft — and returns prioritized, screenshot-grounded fixes formatted as paste-back prompts for Lovable, Bolt, or v0. Use when the user says "review my portfolio", "critique my portfolio site", "review my live portfolio", "look at my portfolio website", "is my portfolio site any good", "/review-portfolio", or pastes a portfolio URL and asks for a critique. Do NOT use to review a case study document or text → use /review-as-recruiter or /review-as-hiring-manager. Do NOT use to build the portfolio prompt → use /build-portfolio. Do NOT use to write or edit case studies → use /generate or /polish.
---

# /review-portfolio — Live Portfolio Site Critique

Visit the user's deployed portfolio in a real browser and critique what actually renders — not a document, the live site. Review it through the same recruiter and hiring manager lenses the rest of this product uses, extended to the website surfaces the case-study frameworks never covered (hero, about, contact, navigation, side content, trust, UI/UX craft).

This runs in the main thread (not a sub-agent) on purpose: the critique depends on directly seeing the rendered screenshots, which a sub-agent could only relay as text.

## Critical — Read First

- **This skill requires the Playwright MCP and a live, public URL.** If the `mcp__playwright__*` browser tools are not available in this session, stop and tell the user: "This review needs the Playwright MCP active. It's configured but only loads at session start — restart Claude Code in this folder, then re-run /review-portfolio." Do not attempt to critique from memory or from the build-portfolio prompt instead.
- **Every finding must cite something observed** in a screenshot, the accessibility tree, or computed styles. If a finding would be true of any website, cut it. Generic web-UX advice is the failure mode this skill exists to avoid.
- **Stay anchored to the target role.** Read it from `context-library/writing-preferences.md`. Every finding answers: "would the recruiter or hiring manager for THIS user's target role care?" If not, drop it.
- **Do not invent page content.** Only critique what rendered. If a section is missing (e.g. no contact path), flag the absence — do not assume it exists elsewhere.

## Step 0: Read Before You Start

| Source | Path | What to extract |
|--------|------|-----------------|
| Evaluation rubric | `insider-data/pm-frameworks/web-portfolio-evaluation.md` | The two lenses, the surface-by-surface checklist, the two scoring axes, and the paste-back output format — this is the backbone of the review |
| Recruiter/HM scan | `insider-data/pm-frameworks/evaluation-frameworks.md` | The 6-second scan criteria — apply to the rendered hero / above-the-fold |
| Target role | `context-library/writing-preferences.md` | Role type and seniority to calibrate every finding; if blank, judge against a generalist PM role and say so |
| Positioning | `context-library/experience-library.md` (if present) | The user's intended positioning, to check hero + about + project selection cohere |

Do not produce a critique until Step 0 and the capture in Step 2 are complete.

## Step 1: Resolve the URL

1. If the user pasted a URL, use it.
2. If not, ask: "What's the live URL of your portfolio? (a public Lovable, Bolt, v0, Vercel, or custom-domain link)." Then stop and wait.
3. Confirm the URL is reachable in Step 2 before reviewing. If it fails to load, report that and stop — do not critique a blank page.

## Step 2: Capture the Site

Use the Playwright MCP tools. Capture all of the following before writing any critique:

1. `mcp__playwright__browser_resize` to 1440x900, then `mcp__playwright__browser_navigate` to the URL.
2. `mcp__playwright__browser_take_screenshot` — above-the-fold (default viewport), then full page (`fullPage: true`). Save to `outputs/portfolio-reviews/assets/`.
3. `mcp__playwright__browser_snapshot` — capture the accessibility tree (heading order, alt text, link/button labels, landmark structure).
4. `mcp__playwright__browser_resize` to 390x844, reload, and `browser_take_screenshot` above-the-fold + full page (mobile). Check for horizontal scroll and overflow.
5. `mcp__playwright__browser_evaluate` — pull base body font-size, hero text color vs. background (for a rough contrast read), and the count of h1/h2 headings.
6. `mcp__playwright__browser_console_messages` and `mcp__playwright__browser_network_requests` — flag broken images, failed requests, or console errors that indicate a broken or half-loaded page.

If the site has multiple pages (case study detail links), navigate into at least one case study to check the entry-point → detail flow.

## Step 3: Run the Two Lenses

Apply `web-portfolio-evaluation.md` against the captured artifacts:

- **Lens 1 — Recruiter 6-second scan** on the rendered above-the-fold (desktop AND mobile): who is this, what kind of PM, is there proof, where do I go next.
- **Lens 2 — Whole-site surfaces:** hero, nav/IA, case study entry cards, about, contact/CTA, side content (signal vs. noise for the target role), social proof, trust breakers.
- **Lens 3 — UI/UX craft:** hierarchy, typography, whitespace, contrast/accessibility, mobile responsiveness, load integrity.

## Step 4: Score — Two Axes, Separate

Score per the rubric, never collapsing the two:
- **Axis A — Portfolio Purpose** (/50): above-the-fold orientation, positioning coherence, proof accessibility.
- **Axis B — UI/UX Craft** (/50): hierarchy+typography+whitespace, mobile, accessibility+trust.

Lead with a single recruiter verdict: would this move forward for [target role] — yes / borderline / no — and the one reason.

## Step 5: Output

Follow the rubric's paste-back format. Save the review to `outputs/portfolio-reviews/[YYYY-MM-DD].md` (version `-v2`, `-v3` if one exists for today) and return it in the chat.

```
=== PORTFOLIO REVIEW: [url] ===
Reviewed as: [target role] · [date]

VERDICT: [Yes / Borderline / No] for [target role] — [one-line reason]

SCORES
Axis A — Portfolio Purpose:  XX/50
Axis B — UI/UX Craft:        XX/50

TOP FIXES (ordered by impact)
1. FINDING: [what's wrong, citing the viewport/element observed]
   WHY IT MATTERS: [recruiter/HM impact for the target role]
   PASTE THIS INTO YOUR VIBE-CODING TOOL:
   "[concrete, self-contained instruction to paste into Lovable/Bolt/v0]"
2. ...
[cap at 5-7 fixes — above-the-fold and trust breakers first, polish last]

WHAT'S WORKING (keep these)
- [2-3 specific strengths, so the user doesn't break them]
```

## Out of Scope

This skill does NOT handle:
- Reviewing a case study document or pasted text → use `/review-as-recruiter` (fast scan) or `/review-as-hiring-manager` (deep read)
- Generating the portfolio build prompt → use `/build-portfolio`
- Writing, editing, or polishing case study content → use `/generate` or `/polish`
- A site that isn't deployed yet (no public URL) → build and deploy first via `/build-portfolio`, then return here

## Cross-Skill Routing

- If a finding is about case study CONTENT (weak outcome, vague ownership) rather than the site → point the user to `/polish` or `/review-as-hiring-manager` on that case study.
- If the positioning itself is the problem (wrong story for the target role) → suggest revisiting `writing-preferences.md` and the career narrative.
- After fixes are pasted and the site is rebuilt → offer to re-run `/review-portfolio` on the updated URL to confirm the changes landed.

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "Playwright tools aren't loaded, I'll critique from the build-portfolio prompt instead" | The prompt is not the rendered site. Vibe-coding tools change layout, hierarchy, and mobile behavior. Reviewing the prompt defeats the skill. Stop and ask for a restart. |
| "I'll review desktop only — mobile is the same" | Recruiters open links on phones constantly, and vibe-coding output breaks most often on mobile. Capture 390px every time. |
| "I can describe good UX generally without citing the screenshot" | Generic advice is the exact failure this skill avoids. Every finding cites an observed element or it gets cut. |
| "No target role in writing-preferences, so I'll judge generically" | Tell the user you're judging against a generalist PM role, then still calibrate to that. Don't silently drop the role lens. |
| "The case study text is weak, I'll rewrite it here" | This skill reviews the SITE. Route content problems to /polish or the HM review. |

## Before Marking Complete

- [ ] Playwright tools confirmed available (or stopped and asked for restart)
- [ ] Step 0 rubric + target role read (named which)
- [ ] Live URL loaded successfully; not a blank/broken page
- [ ] Captured: desktop above-fold + full, mobile above-fold + full, accessibility tree, contrast/heading data, console/network check
- [ ] At least one case study detail page visited if links exist
- [ ] Every finding cites an observed element — no generic advice
- [ ] Scored both axes separately with a top-line recruiter verdict
- [ ] Top fixes formatted as paste-back prompts, capped at 5-7
- [ ] Review saved to `outputs/portfolio-reviews/[YYYY-MM-DD].md`
