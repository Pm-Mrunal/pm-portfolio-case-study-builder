# Web Portfolio Evaluation Framework 

> Evaluates a LIVE, rendered portfolio website — not a case study document.
> Fills the gap the existing frameworks don't cover: the website surface around the case studies
> (hero, about, contact, navigation, side content, trust, and UI/UX craft).
> Pair with `evaluation-frameworks.md` (the 6-second scan + HM lens transfer directly to the rendered hero and positioning).
> STATUS: draft, under POC validation. If the critique it produces is specific and useful, promote to a permanent framework.

---

## How to use this

Drive the live URL with the Playwright MCP. Capture, at minimum:
- Desktop screenshot at 1440px wide (full page + above-the-fold crop)
- Mobile screenshot at 390px wide (full page + above-the-fold crop)
- Accessibility tree / DOM (heading order, alt text, link labels, button labels)
- Computed styles for contrast + base font size on body and hero
- Network/load behavior (does anything stay blank, slow, or broken)

Every finding must cite something actually observed in a screenshot or the DOM. No generic web-UX advice that could apply to any site — if it isn't grounded in this site, cut it.

---

## Lens 1 — The Recruiter 6-Second Scan (on the rendered site)

Reuses the scan logic from `evaluation-frameworks.md`, applied to the hero / above-the-fold as it actually renders.

In the first viewport, before any scroll, can a recruiter answer:
1. **Who is this?** Name + role + seniority visible immediately (matches target: see `writing-preferences.md`)
2. **What kind of PM?** Positioning/specialization clear in one line (AI PM, Growth, 0-to-1, etc.)
3. **Is there proof?** At least one case study or outcome reachable/visible without hunting
4. **Where do I go next?** An obvious primary action (view work / contact / resume)

If the above-the-fold fails any of these, that is the highest-priority finding. Recruiters do not scroll a portfolio that doesn't orient them in the first screen.

---

## Lens 2 — The Whole-Site Surfaces (what the case-study frameworks miss)

Evaluate each surface that exists. Flag surfaces that are missing but expected.

| Surface | What good looks like | Common failure to flag |
|---|---|---|
| **Hero / above-the-fold** | One-line positioning, role+level signal, primary CTA, proof in/near first screen | Generic tagline ("Product Manager passionate about building"), no level signal, no CTA, hero is a wall of prose |
| **Navigation / IA** | Case study reachable in 1 click; clear labels; sticky or obvious nav | Buried case studies, vague labels ("Stuff"), no nav on mobile, anchor links that jump past content |
| **Case study entry points (cards)** | Each card leads with outcome/role + a hook; scannable; consistent | Cards lead with company logo only, no outcome, walls of text, inconsistent lengths |
| **About** | Credible narrative, human signal (photo ok), positioning reinforced, concise | Life story, no positioning, contradicts the hero, missing entirely |
| **Contact / CTA** | Obvious way to reach out (email/LinkedIn/form), present site-wide | No contact path at all (recruiter dealbreaker), dead mailto, contact only at very bottom |
| **Side content (projects / blog)** | Reinforces the target positioning; clearly secondary | Dilutes positioning (random hackathons for a Senior AI PM), competes with case studies for attention, stale/empty blog |
| **Social proof** | LinkedIn, resume download, recognizable logos, references | None present; broken resume link; logos with no context |
| **Trust / professionalism** | No placeholder text, no dead links/buttons, consistent polish | Lorem ipsum, "[Your Name]", dead buttons, broken images, 404 links, vibe-coding-tool default text left in |

---

## Lens 3 — UI/UX Craft (not covered anywhere in existing frameworks)

Score on observable craft, grounded in the screenshots and computed styles:

- **Visual hierarchy** — does the eye land on the right thing first (name/positioning, then proof)?
- **Typography** — readable base size (>=16px body), limited type scale, consistent
- **Whitespace & density** — breathing room vs. cramped walls of text
- **Contrast & accessibility** — body/hero contrast ratio >= 4.5:1; headings in order (one h1, logical h2s); images have alt text; links/buttons have labels
- **Mobile responsiveness** — at 390px: no horizontal scroll, no overflowing text, nav works, tap targets reasonable
- **Load & integrity** — nothing stays blank, no broken images, no console-level breakage visible on the page

---

## Scoring — two axes, kept separate

Do not collapse these. A site can be beautiful and fail its purpose, or land its purpose while looking junior.

**Axis A — Portfolio Purpose (does it get an interview for the TARGET role)**  — 0-50
- Above-the-fold orientation (Lens 1) /20
- Positioning coherence across hero + about + project selection /15
- Proof accessibility — can the recruiter reach a strong case study fast /15

**Axis B — UI/UX Craft (does it look like a credible professional)** — 0-50
- Visual hierarchy + typography + whitespace /20
- Mobile responsiveness /15
- Accessibility + trust/integrity (no breakers) /15

Report both subtotals and a one-line verdict per axis. A recruiter-facing verdict at top: *would this move forward for [target role], yes / borderline / no, and the single reason.*

---

## Output format — paste-back fixes, not a report

This is what makes the critique actionable instead of a PDF nobody opens. For each high-priority finding, produce:

```
FINDING: [what's wrong, citing the screenshot/viewport — e.g. "On mobile (390px), the 3rd case study card's title overflows its container"]
WHY IT MATTERS: [tie to recruiter/HM impact for the target role]
PASTE THIS INTO YOUR VIBE-CODING TOOL:
"[a concrete, self-contained instruction the user can paste straight into Lovable/Bolt/v0 to fix it]"
```

Order findings by impact: above-the-fold and trust breakers first, craft polish last. Cap at the top 5-7 so the user actually acts on them.

---

## Boundary (keep this skill from drifting into a generic web audit)

Every finding must answer: "would the recruiter or hiring manager for THIS user's target role care?" If a finding is true of any website but not specific to landing this user an interview, drop it. The differentiation is the role-calibrated recruiter/HM lens — not generic UX heuristics any tool can recite.
