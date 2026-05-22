# Summary — visual-layer Improvement Run

Date: 2026-05-22
Target: `/Users/mrunalsurve/Documents/pm-case-study-builder/.claude/skills/visual-layer/SKILL.md`
Domain: PM / Post-processing (visual planning + placeholder insertion + caption generation)

---

## Before / After

| | Original | Improved |
|---|---|---|
| Input 01 (direct invocation) | 72/100 B | 90/100 A+ |
| Input 02 (trigger phrase) | 82/100 A | 90/100 A+ |
| Input 03 (adjacent NL) | 70/100 B | 90/100 A+ |
| **Average** | **74.7/100 B** | **90.0/100 A+** |

Total iterations: 1

---

## What Improved

**Highest-leverage fix: Description rewrite + Step 0 input detection gate**

The original description had WHAT but zero trigger phrases and no "Do NOT use for" boundary. All trigger language ("add visuals", "add visual placeholders") was buried in a "## When to Use" body section — invisible to the router at startup. Adjacent-NL inputs like "make it more visual" would not reliably invoke the skill.

Adding 7 explicit trigger phrases to the description and removing the body "When to Use" section moved routing quality from 9.3/20 avg to 18.0/20 avg — accounting for ~57% of the total gain.

**Second fix: "Already applied" detection in Step 0**

The original skill had no instruction for the case where the visual layer was already present in the output file. The original agent improvised a reasonable response (markdown table, offer to re-run) but in a non-standard format that varied from run to run. Adding an explicit "already applied" branch to Step 0 made this path deterministic and consistent, fixing output format consistency from 14.7 to 18.0 avg.

**Third fix: Anti-rationalization table**

5 rows covering the specific shortcuts this 4-step skill is prone to: inferring content from project name (instead of reading the file), silently adding visuals when layer already applied, skipping captions, exceeding TLDR placeholder limit, describing Step 4 instead of executing it.

---

## What Was Already Good

- Visual plan JSON template: complete and specific, no changes needed
- Placeholder format and rules: correct, no changes needed
- Caption rules and template: correct, no changes needed
- Step 4 assembly format: correct file structure, no changes needed
- Non-fabrication rules: strong — "exists_in_inputs: false" default, no tool references unless user-stated
- Quality checks: comprehensive list — restructured from post-save advisory to pre-save mandatory gate

---

## Files

```
outputs/improve-skill-runs/visual-layer-2026-05-22/
├── summary.md              ✅
├── rubric.md               ✅
├── original-skill.md       ✅
├── improved-skill.md       ✅
├── inputs/
│   ├── 01.md               ✅
│   ├── 02.md               ✅
│   └── 03.md               ✅
├── outputs-original/
│   ├── 01.md               ✅
│   ├── 02.md               ✅
│   └── 03.md               ✅
├── outputs-improved/
│   ├── 01.md               ✅
│   ├── 02.md               ✅
│   └── 03.md               ✅
├── grades-original.md      ✅
├── grades-improved.md      ✅
├── iterations/
│   └── iter-1-skill.md     ✅
└── iterations.md           ✅
```
