# Original Skill Grades

Skill: `/Users/mrunalsurve/Documents/pm-case-study-builder/.claude/skills/visual-layer/SKILL.md`
Date: 2026-05-22

---

## Input 01 — Direct invocation, no context

| Criterion | Score (/20) | Evidence |
|-----------|-------------|----------|
| Routing quality | 10 | Description 217 chars with WHAT but no trigger phrases; "When to Use" in body (invisible to router); direct `/visual-layer` command fires reliably but adjacent-NL will not |
| Output specificity | 16 | Correct principled-decline: listed specific available output files from outputs/ directory; named what was missing; suggested concrete next step. Per rubric anchor, principled-decline scores 14-18. |
| Output format consistency | 14 | Conversational response appropriate for principled-decline path; however "what the skill would do next" description is ad-hoc and would vary across runs (no template for this path) |
| Step completeness | 16 | Correctly stopped at Phase 1 and named what was missing — per rubric anchor this is 14-18, not a failure. No explicit STOP instruction in skill; agent inferred from "When to Use" section |
| Non-fabrication adherence | 16 | Did not fabricate any visuals; correctly deferred; listed real files from directory |
| **Total** | **72/100** | **Grade: B** |

---

## Input 02 — "Add visual placeholders" with file reference

| Criterion | Score (/20) | Evidence |
|-----------|-------------|----------|
| Routing quality | 10 | "add visual placeholders" is in the body "When to Use" section — invisible to the router at description time. Agent fired successfully but routing is not reliable in fresh sessions without full body context |
| Output specificity | 18 | All 6 visuals grounded in real checkout-redesign data: 18% abandonment, 12 users, 5-step flow, security anxiety finding, 6-week timeline, 2-week ahead of schedule, AOV. No training-data defaults. |
| Output format consistency | 18 | visual_plan JSON matches template; placeholder syntax `[VISUAL: type – description]` correct; captions JSON matches template; final file has all 5 sections; all quality checks passed |
| Step completeness | 18 | All 4 steps explicitly executed: visual_plan → placeholders in both TLDR (3) and detailed (6) → captions → assembled and saved to outputs/ |
| Non-fabrication adherence | 18 | All visuals grounded in project schema; exists_in_inputs: false correctly set; no tools referenced; captions describe what visuals would show without inventing claims |
| **Total** | **82/100** | **Grade: A** |

---

## Input 03 — Adjacent NL ("make it more visual for my portfolio")

| Criterion | Score (/20) | Evidence |
|-----------|-------------|----------|
| Routing quality | 8 | No trigger phrases in description; adjacent-NL fired only because CLAUDE.md system context loaded — not from skill description. In a fresh session without CLAUDE.md, this input would NOT reliably invoke the skill |
| Output specificity | 18 | Excellent — all content grounded in actual outflow-ai data (12.4% signup, 61% setup completion, 43% action rate, specific pipeline stages). Real captions with real numbers. |
| Output format consistency | 12 | Format deviates: output is markdown table, NOT the JSON visual_plan/visual_captions templates. Skill has no instruction for the "already applied" case, so agent improvised in a non-standard format |
| Step completeness | 14 | Pipeline not re-run (correct — layer already applied to file). Skill has no explicit instruction for re-run detection, so agent improvised correctly but inconsistently |
| Non-fabrication adherence | 18 | All data from actual project file; exists_in_inputs correctly inferred; captions only describe what visuals would show |
| **Total** | **70/100** | **Grade: B** |

---

## Summary

| Input | Total | Grade |
|-------|-------|-------|
| Input 01 (direct invocation) | 72/100 | B |
| Input 02 (trigger phrase + file) | 82/100 | A |
| Input 03 (adjacent NL) | 70/100 | B |
| **Average** | **74.7/100** | **B** |

---

## Lowest-Scoring Criterion

**Routing quality** — avg 9.3/20 across all inputs.

Root cause: description has WHAT but zero trigger phrases; "When to Use" section with all trigger language is buried in the body (invisible to the router). Secondary failure: no "Do NOT use for" boundary in description.

**Second-lowest: Output format consistency** — avg 14.7/20, driven by Input 03's "already applied" case where the skill has no instruction path.

---

## Structural Failures Driving Low Scores

1. No trigger phrases in description — "When to Use" body section is routing-invisible
2. "What This Skill Does NOT Do" has no → /Y pointers
3. No Step 0 with explicit input detection (principled-decline relies on inference, not instruction)
4. No instruction for "already applied" re-run case
5. No anti-rationalization table for a 4-step skill
6. "Quality Checks Before Finishing" is post-pipeline advisory, not a pre-save gate
