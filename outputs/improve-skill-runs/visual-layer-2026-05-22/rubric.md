# Rubric — visual-layer

Target: `/Users/mrunalsurve/Documents/pm-case-study-builder/.claude/skills/visual-layer/SKILL.md`
Date: 2026-05-22

---

## Criteria (5 × 20 = 100 pts)

### 1. Routing Quality (universal) — /20

Does Claude reliably invoke this skill when a user asks to add visuals to a generated case study?

| Score | Anchor |
|-------|--------|
| 20 | Description 200+ chars, WHAT+WHEN in first 250, 3+ explicit user-typed trigger phrases, "Do NOT use for" boundary with → /Y pointers, third person |
| 16–18 | Strong description but missing 1 element (e.g., has triggers but no boundary pointers) |
| 10–14 | WHAT present, WHEN vague, 1 trigger phrase or none; "When to Use" buried in body |
| 4–8 | Short description, no trigger phrases, no boundary; body When-to-Use section (invisible to router) |
| 0–2 | No frontmatter, or description under 50 chars |

### 2. Output Specificity (universal) — /20

Does the output use specific project data (named steps, real metrics, actual workflow facts from the case study) or does it read like generic visual suggestions?

| Score | Anchor |
|-------|--------|
| 20 | Every visual in the plan cites specific project facts (named sections, real drop-off stats, actual feature names); captions reference specific evidence from the case study |
| 16–18 | Mostly grounded, 1-2 generic visual recommendations mixed in |
| 10–14 | Mix of specific and generic; some visuals are project-grounded, others are training-data defaults |
| 4–8 | Primarily generic ("add a chart showing metrics", "user flow diagram"); minimal project grounding |
| 0–2 | Entirely fabricated from training data; no reference to actual case study content |

**Rubric anchor — principled-decline path:**
When Input 1 (/visual-layer with no case study provided) correctly stops and asks for the output file, score:
- Output specificity: 14–18 (correct decline naming the missing input)
- Step completeness: 14–18 (Phase 1 only — correct behavior, not a failure)
Do NOT penalize correct principled-decline behavior.

### 3. Output Format Consistency (universal) — /20

Would three runs of the same input produce the same output shape?

| Score | Anchor |
|-------|--------|
| 20 | `visual_plan` JSON matches template exactly; placeholder syntax `[VISUAL: type – description]` used throughout; `visual_captions` JSON matches template; final file has all 5 sections (RECRUITER TLDR, DETAILED HIRING MANAGER VERSION, PORTFOLIO ASSETS, VISUAL PLAN, VISUAL CAPTIONS) |
| 16–18 | Correct JSON structure; minor deviations (e.g., captions in markdown instead of JSON, or 4 of 5 file sections) |
| 10–14 | Outputs exist but format varies from template (e.g., visual plan as markdown list instead of JSON) |
| 4–8 | No structured JSON; placeholders formatted differently than spec |
| 0–2 | No structured output whatsoever |

### 4. Step Completeness (domain primary) — /20

Does the skill execute all 4 steps in order and produce each expected output?

| Score | Anchor |
|-------|--------|
| 20 | Step 1 produces `visual_plan` JSON → Step 2 inserts placeholders into both TLDR and detailed version → Step 3 produces `visual_captions` JSON → Step 4 assembles all outputs into final file and saves |
| 16–18 | 3 of 4 steps explicitly complete; one step partially merged or silently skipped |
| 10–14 | 2 of 4 steps visible; pipeline stops early or skips Step 2 or 3 |
| 4–8 | Only 1 step complete (e.g., only visual_plan without placeholders or captions) |
| 0–2 | Pipeline not run; Claude responds conversationally without executing steps |

### 5. Non-Fabrication Adherence (domain secondary) — /20

Are all visual recommendations grounded in project data, not invented from training data? Are `exists_in_inputs` flags correct?

| Score | Anchor |
|-------|--------|
| 20 | All visuals cite specific project content; `exists_in_inputs: false` when no visuals mentioned in source; no tools (Figma, Miro, etc.) referenced unless user stated them; captions describe what the visual *would show* without inventing claims |
| 16–18 | Mostly grounded; 1 visual feels generic but isn't fabricated; `exists_in_inputs` correctly set |
| 10–14 | 2-3 generic visuals that could apply to any PM case study; or 1 invented tool reference |
| 4–8 | Multiple fabricated visuals; `exists_in_inputs: true` when user never mentioned images; invented metric claims in captions |
| 0–2 | Entirely fabricated from PM training data; captions make up content not in source |

---

## Grade Scale

| Score | Grade |
|-------|-------|
| 90–100 | A+ |
| 80–89 | A |
| 70–79 | B |
| 60–69 | C |
| < 60 | F |
