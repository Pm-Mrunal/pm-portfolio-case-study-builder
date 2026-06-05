---
name: improve-skill
description: Improves any Claude skill — PM, job-search, content, personal, engineering, anything — by running an evaluation loop on it. Builds a custom rubric for the target skill, generates realistic test inputs, runs the inputs through the original skill, scores the outputs, edits the SKILL.md to fix the lowest-scoring criteria, re-runs the inputs through the improved skill, and loops until the improved version scores A+ across the rubric. Produces full proof of work — the rubric, the test inputs, the original outputs, the improved outputs, the iteration log, and the grade table — saved to `outputs/`. Use when the user types 'improve skill', 'improve this skill', 'audit skill', 'fix my skill', '10x this skill', 'review skill', '/improve-skill', or points to a SKILL.md and asks to upgrade it. Do NOT use to create a new skill from scratch — write one manually using `references/twelve-tips.md` as a template. Do NOT use for tuning CLAUDE.md or hooks — use /update-config instead. Do NOT use to grade a one-off prompt or general writing.
---

# improve-skill

You improve any Claude skill — PM, job-search, content, personal, engineering, finance, fitness, whatever — by running an empirical evaluation loop on it. The loop is the core methodology. The 12 tips and the 11-item audit checklist are diagnostics inside the loop, not the prescription.

The principle: a "rule-applied" rewrite is hypothesis. A re-run on real inputs with grades that improve is proof.

## Critical

- **Run the eval loop. Do not just apply tips.** Tips are hypotheses. The eval is the test. If a tip-based edit doesn't move the rubric, undo it.
- **Read the actual target SKILL.md.** Never edit from training data, summarized memory, or the conversation transcript.
- **Edit in place.** Do not produce a "suggestions doc". The output is the rewritten SKILL.md plus a proof-of-work artifact.
- **Domain-agnostic.** Generate the rubric and test inputs from *this skill's* description. Don't assume PM. The same loop works on a resume rewriter, a recipe planner, a debug-trace skill, a fitness coach, an investor update.
- **One skill, one job.** If the target skill is doing four things, flag it as a split candidate before iterating. For multi-job skills, the first intervention is router logic, not content refinement.
- **Critical instructions stay at the top.** When you add new sections, do not bury existing `## Critical` blocks.
- **Score description and body independently.** A 900-line "gold-standard" body with a 36-char description is a routing failure, not a polished skill. Always check description char-count + trigger-phrase count BEFORE reading the body. (Empirical: 4 of 4 PM OS "gold-standard" skills had this exact pattern.)
- **For health/safety/medical/financial domains, safety-critical rules go in the FIRST `## block`.** Body length is not the diagnostic — placement is. If the skill author wrote "I keep forgetting to add these at the top," they ARE being violated. Move them.
- **Verify referenced files exist.** Skills that reference `${CLAUDE_SKILL_DIR}/...` or `references/X.md` files that don't exist fail silently every run. Audit must check.
- **Verify YAML name matches folder name.** Mismatch means `/folder-name` invocation hits nothing — silent routing failure distinct from missing frontmatter.

## Step 0: Read Before You Write

Read these in this order:

| Source | Path | What to extract |
|--------|------|-----------------|
| Target skill | `<user-supplied path>/SKILL.md` | Frontmatter (name, description), every body section, total line count, names of trigger phrases, current out-of-scope language |
| Target skill references | `<path>/references/*.md` | Any existing reference content the skill loads on demand |
| Twelve tips | `references/twelve-tips.md` | Diagnostic patterns — match against rubric failures |
| Audit checklist | `references/audit-checklist.md` | Quick scan for common gaps |
| Worked examples | `references/examples.md` | Before/after shapes for description, context routing, output format |
| Past learnings | `references/learnings.md` | Patterns flagged successful (apply) or failed (avoid) in prior runs |
| Sibling skills | `<skills-dir>/*/SKILL.md` (frontmatter only) | Names + descriptions of nearby skills, for cross-skill routing pointers |

If `references/learnings.md` does not exist, skip it. Do not invent learnings.

If the user did not supply a skill path, ask once: "Which SKILL.md should I improve? (path or skill folder name)". Do not guess.

Only proceed past Step 0 once the target file is read.

## Step 1: Build a Rubric for THIS Skill

Read the target skill's description and body. Write a 5-criterion rubric, **20 points each, /100 total**. The first three criteria are universal. The last two are domain-specific to *this skill*.

Universal criteria (always include):

1. **Routing quality (description)** — would Claude actually invoke this skill on relevant prompts? Score against: 100+ chars, WHAT+WHEN in first 250, 3+ trigger phrases, "Do NOT use for" boundary, third person, pushy phrasing.
2. **Output specificity** — does the output use specific context (named files, real numbers, named stakeholders, actual user inputs) or does it read like generic training-data output?
3. **Output format consistency** — would three runs of the same input produce the same shape? Or does Claude re-invent the format each time?

**Domain criteria selection table** (pick 2 — primary + secondary — based on what the skill is for):

| Domain | Primary criterion (rec) | Secondary criterion (rec) |
|--------|-------------------------|---------------------------|
| Analysis (activation, retention, funnel) | *evidence trail* (cites which file each claim came from) | *actionability* (recommendation has owner + deadline + measurable outcome) |
| Writing — outreach (cover letter, referral, recruiter msg) | *personalization* (mentions specifics about the recipient) | *call-to-action* (single, specific ask — not "thoughts?") |
| Writing — content (LinkedIn, tweet, newsletter) | *voice match* (matches user's voice, not generic LLM-prose) | *hook strength* (first line passes scroll-stop test) |
| Writing — professional (status update, investor update) | *signal density* (wins/blockers/asks, not stuff-I-did) | *honesty flagging* (surfaces bad news, doesn't bury it) |
| Planning (daily plan, sprint plan, training block) | *prioritization* (impact × effort, not arbitrary) | *graceful degradation* (works when MCPs/files missing) |
| Code review / debugging | *file:line precision* (concrete refs) | *severity tagging* (Critical/High/Med/Low explicit) |
| Code authoring (RFC, commit msg, refactor) | *correctness / type-correctness* | *risk flagging* / *alternatives-not-strawmen* |
| Research / market data | *primary-source ratio* (cites real sources) | *triangulation* (2+ independent sources for key claims) |
| Synthesis / summary | *coverage* (every input addressed) | *signal/noise* (separates what matters from trivia) |
| Decision (build vs buy, PRD vs no-PRD) | *opinion* (clear recommendation) | *kill criteria* (states what would change the decision) |
| Education / tutoring | *checkpoints* (verifies understanding before advancing) | *progression* (basics → advanced, no skipped prereqs) |
| Health / safety / fitness | *safety-first application* (rules fire before prescription) | *individualization* (respects user's stated constraints/injuries) |
| Finance / budget | *math correctness* (totals add up) | *category completeness* (fixed + variable + savings) |
| Outreach with copy-paste artifacts (referral msg, intro email) | *personalization* | *placeholder scrubbing* (no `[Your Name]` or `[Company]` in the deliverable) |
| Multi-step provisioning / setup (Arize, infra, scaffolding) | *step completeness* (every step actually executed) | *id-tracking* (multi-resource IDs scaffolded) |
| Cooking / recipes | *constraint respect* (dietary/time/equipment) | *substitution flexibility* / *contradiction flagging* |
| Scaffolding (second brain, repo init) | *idempotency* (re-run doesn't clobber existing) | *starter completeness* (output is a real starting point, not stubs) |

If the skill doesn't fit a row, write your own — the table is a starting point, not a constraint.

Save the rubric to `outputs/improve-skill-runs/<skill-name>-<YYYY-MM-DD>/rubric.md`.

**Rubric anchor — principled-decline path.** When a test input has no required context (e.g., direct invocation `/skill` with no args, or input where the skill's needed files don't exist), the *correct* output is "stop, name what's missing, suggest a next step" — not fabrication. A skill that does this gets:
- Output specificity: 14–18/20 (NOT 4/20 — it correctly declines)
- Domain primary: 14–18/20 if the decline names the missing input
- Other criteria: scored as if Phase-1-only output is the deliverable

Without this anchor, scoring penalizes correct behavior and caps the skill at ~84. Add a worked Phase-1 example to the skill so Phase-2 is also visible to the rubric.

## Step 2: Generate 3 Test Inputs

Generate three realistic prompts a real user would type to invoke this skill. Vary them across the trigger surface:

- **Input 1: direct invocation** — `/skill-name` or "use the skill-name skill"
- **Input 2: trigger phrase** — a phrase from the description that should fire the skill ("plan my day", "rewrite my resume", "create tickets")
- **Input 3: adjacent natural-language** — a request that *should* fire the skill but doesn't quote any trigger phrase verbatim (tests whether the description has enough latent surface)

Optionally **Input 4: out-of-scope** — a request that should NOT fire the skill, to test whether negative boundaries hold. Score 0 if the skill answers it; 20 if the skill correctly declines or routes elsewhere.

Save inputs to `outputs/improve-skill-runs/<skill-name>-<YYYY-MM-DD>/inputs/01.md`, `02.md`, `03.md` (and `04.md` if used).

## Step 3: Run Each Input Through the ORIGINAL Skill

For each input, follow the skill's instructions *as written*. Don't fix the skill yet. Capture exactly what Claude would produce given the skill's current state. If the skill names files that don't exist, write what Claude would do (read from training data, hallucinate, etc.) — that *is* the failure mode.

Use the Agent tool with subagent_type=general-purpose. The agent prompt:

```
You are running a Claude skill in evaluation mode. Read the skill at <path>/SKILL.md and follow its instructions exactly. Do not improve it. Do not skip steps. Do not add commentary. The user input is:

<input text>

Produce the output the skill would produce. If the skill calls for reading files that don't exist, note that in the output.
```

Save each output to `outputs/.../outputs-original/01.md`, `02.md`, `03.md`.

## Step 4: Score the Original Against the Rubric

For each input/output pair, score each criterion 0–20. Total /100. A+ = 90+. A = 80–89. B = 70–79. C = under 70.

Identify the **lowest-scoring criterion across all 3 inputs** — that's the highest-leverage improvement target. Also note which SKILL.md section drove the low score (description? Step 0? output format? missing exit checklist?).

Save grades to `outputs/.../grades-original.md` with this exact table per input:

| Criterion | Score (/20) | Evidence |
|-----------|-------------|----------|
| Routing quality | X | [why] |
| Output specificity | X | [why] |
| Output format consistency | X | [why] |
| [Domain criterion 1] | X | [why] |
| [Domain criterion 2] | X | [why] |
| **Total** | **/100** | Grade: A+ / A / B / C |

## Step 5: Improve the Skill (First Principles, Not Tip-Cargo-Cult)

For each low-scoring criterion, identify the *specific failure mode* in the current SKILL.md, then apply the relevant tip to fix it. The tips are tools; the rubric failure is the diagnosis.

Mapping (use as guide, not script):

| Rubric failure | Likely root cause | Apply |
|----------------|-------------------|-------|
| Routing quality low | Description too short / no triggers / no boundary / first person / YAML name doesn't match folder name | Tip 1 + Tip 2 (and verify name == folder) |
| Routing quality low + skill has no frontmatter | Missing YAML | Add frontmatter — single 5-line fix moves score 70+ pts (cf. budget-helper 13→89, li-skill-rough 0→20 routing) |
| Output specificity low + zero context | No Step 0 read-first OR Step 0 doesn't name files | Tip 4 (Source/Path/Extract table) |
| Output specificity low + correct decline observed | This is the *principled-decline path*; score per anchor in Step 1 — don't penalize | Add a worked Phase-1 + Phase-2 example so the full pipeline is rubric-visible (Tip 10) |
| Output specificity low + outputs use plausible defaults as facts | "Inferred defaults masquerading as user-supplied facts" (Tip 13) | Add `[DEFAULT: X — confirm with user]` markers in Step 0 for any inferred infra/SLO/equity/level value |
| Output format consistency low | No template, or rules instead of worked example | Tip 5 + Tip 10 (for skills with multiple valid sub-shapes — bullets vs prose, body vs no body — rules alone never resolve them; worked examples do) |
| Step-skipping observed | No anti-rationalization table | Tip 11 |
| Partial output / unfinished work | No exit checklist | Tip 12 |
| Output drifts into adjacent topics | No out-of-scope section, or exclusions without pointers | Tip 6 — but put the exclusions in the *description* (with → use /Y), not just the body. Body exclusions are advisory; description exclusions fire at routing time. |
| Vague/conversational instructions ↔ vague/conversational output (1:1 propagation) | Body uses softening language | Tip 3 + Tip 11 — softening propagates symmetrically. Imperative rewrite is functional, not cosmetic. |
| SKILL.md too long, late instructions ignored | Body over 500 lines AND content is duplicated rules / buried critical instructions | Tip 8 (move detail to references/, one level deep) |
| SKILL.md too long, but content is worked examples | Worked examples are the specificity driver for content/writing skills | DO NOT trim. The 500-line guideline relaxes when excess lines are worked examples (Tip 10 gold pattern). Move only reference data tables, not examples. |
| Health/safety/medical/financial domain + critical rules buried late in body | Bloat-as-safety-hazard | Move safety-critical rules to FIRST `## block`, before Step 0, regardless of body length. |
| Skill produces standalone output but adjacent issues need handling | No cross-skill routing | Tip 7 — verify sibling skill exists in folder before pointing to it. Unverified pointers cap routing at 18/20 and break worse than no link. |
| Skill is reused often but doesn't improve | No learnings.md integration | Tip 9 — but skip for one-time scaffolding skills (no compounding value). |
| Skill references files that don't exist (`${VAR}` or relative paths to deleted files) | Broken-pointer silent failure | Embed inline OR add `## Critical` block warning against the path OR create the file. Never ship a reference that fails silently. |
| Outreach skill output contains `[Your Name]` / `[Company]` placeholders | Skill assumed user info would be auto-extracted | Add Step 0 user-identity extraction + exit-checklist gate that blocks output if any `[placeholder]` remains (scrub for `[A-Z][^\]]*\]` regex). |
| Multi-step provisioning skill — users lose track of resource IDs across steps | No ID-tracking scaffold | Add a "Track IDs as you go" block at top of workflow with blank lines per ID. |
| Skill collects constraints but doesn't enforce them (e.g., "asks about diet" but ignores conflicts) | Constraint collection ≠ enforcement | Add explicit Critical-section contradiction-flag rule: state contradiction before proceeding. Silent resolution is a trust failure. |
| First-person body content ("I'll quiz you on...") | Persona-style writing, common in education / tutor / coach skills | Restructure to Mode-step format (Mode A: Step A1/A2/A3) — preserves teaching logic while making imperative. Don't rewrite every "I'll" in isolation. |

**First principles override:** if the rubric reveals a failure mode none of the tips name, write a fix that addresses the failure, not the closest tip. The tips are common patterns, not the universe.

Edit `<path>/SKILL.md` in place using the Edit tool. After each edit, save a snapshot of the new SKILL.md to `outputs/.../iterations/iter-1-skill.md` (so the loop is auditable).

## Step 6: Re-run Each Input Through the IMPROVED Skill

Same agent prompt as Step 3, but pointing at the rewritten skill. Same 3 (or 4) inputs. Same evaluation conditions. Capture outputs to `outputs/.../outputs-improved/01.md`, `02.md`, `03.md`.

## Step 7: Score the Improved Against the Rubric

Same rubric. Same scoring rules. Save to `outputs/.../grades-improved.md`. Compare totals to baseline.

## Step 8: Loop Until A+ — or Stop if You Can't Move the Score

If any input scored under 90/100 on the improved version:

1. Identify which criterion regressed or stayed low.
2. Re-read the SKILL.md section that should have addressed it.
3. Make a targeted edit (one criterion at a time — don't re-touch the whole skill).
4. Save a new snapshot to `outputs/.../iterations/iter-N-skill.md`.
5. Re-run the failing input(s) only — no need to re-run inputs already at A+.
6. Re-score.

Stop conditions:
- All inputs score A+ (90+) → success.
- 3 iterations completed and score hasn't moved → stop, document what's blocking.
- Score regressed → roll back to previous iteration, document what didn't work.

Save the iteration log to `outputs/.../iterations.md`.

## Step 9: Produce the Proof-of-Work Artifact

Final folder structure:

```
outputs/improve-skill-runs/<skill-name>-<YYYY-MM-DD>/
├── summary.md              ← top-level: what improved, before/after grades, total iterations
├── rubric.md               ← the 5 criteria with 0–20 anchors per criterion
├── original-skill.md       ← snapshot of SKILL.md before any edits
├── improved-skill.md       ← snapshot of final SKILL.md
├── inputs/
│   ├── 01.md, 02.md, 03.md (and 04.md if used)
├── outputs-original/
│   ├── 01.md, 02.md, 03.md
├── outputs-improved/
│   ├── 01.md, 02.md, 03.md
├── grades-original.md
├── grades-improved.md
├── iterations/
│   ├── iter-1-skill.md, iter-2-skill.md (only if loops happened)
└── iterations.md           ← per-iteration: what changed, what moved on the rubric
```

The user can audit any link in the chain.

## Out of Scope

This skill does NOT handle:
- Creating a new skill from scratch → write one manually using `references/twelve-tips.md` as a template, or use `/init` to scaffold a project
- Tuning CLAUDE.md project instructions → those are not skills; edit CLAUDE.md directly
- Building hooks or modifying settings.json → use `/update-config`
- Setting up recurring runs of a skill → use `/schedule` or `/loop`
- Reviewing arbitrary code or PRs → use `/review` or `/security-review`
- Grading a one-off prompt → run the prompt yourself; this skill is for iterating *on a skill that gets reused*
- Renaming a skill or restructuring a multi-skill folder → out of scope; ask the user first

## Cross-Skill Routing

After improving:
- If the rewrite revealed the skill should fire automatically (post-edit, on-stop) → recommend `/update-config` to add a hook
- If the rewrite revealed the skill duplicates another in the same folder → recommend merging
- If the user wants the skill to run on a schedule → recommend `/schedule`

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|----------------------|----------------|
| "I have the skill content from earlier in the conversation, I can skip Step 0" | Step 0 reads the file's *current* state. Working from transcript means working from stale or fabricated content. |
| "I can apply the 12 tips and skip the eval — it's well-known what good looks like" | Tips are hypothesis. The eval is proof. A description that *looks* good can still route badly. The loop catches this. |
| "Three test inputs is overkill; one is enough" | One input is anecdote. Three reveals whether the skill is consistently better, or just better on a friendly case. |
| "The output format is obvious — I don't need a worked example in the rubric" | "Obvious" means "I'm filling in the gap". Score it as if a stranger were grading. |
| "I'll write up suggestions and let the user apply them" | The skill rewrites in place. A suggestions doc is one more thing the user has to do — and they won't. |
| "If iteration 1 didn't move the score, the skill is just bad" | Maybe. Or maybe iter-1 fixed criterion A and broke criterion B. Read the per-criterion deltas, not just the total. |
| "I'll add cross-skill links to /retention-analysis and /experiment-metrics — those probably exist" | Read the skills directory first. Linking to skills that don't exist breaks routing harder than no link. |
| "Anti-rationalization table is overkill for a 2-step skill" | True for 2-step. Re-count — numbered lists in the body usually mean 3+ steps. |
| "I'll log to learnings.md if I remember at the end" | The exit checklist requires it. Don't mark complete until logged. |

## Worked Example (skill-agnostic)

Suppose target skill is `cover-letter-writer` (job-search domain) with this description:

```yaml
---
name: cover-letter-writer
description: Write a cover letter
---
```

**Step 1 rubric** (5 criteria for this specific skill):
1. Routing quality (universal)
2. Output specificity (universal)
3. Output format consistency (universal)
4. **Voice match** (domain): does the letter sound like the user, or like generic LLM-prose?
5. **Job-specific personalization** (domain): does it reference the actual JD, company, or role specifics?

**Step 2 inputs:**
- Input 1: `/cover-letter-writer` (direct)
- Input 2: "write a cover letter for the staff PM role at Notion" (trigger phrase)
- Input 3: "I'm applying to Notion for staff PM, can you help me apply?" (adjacent)

**Step 3 original output (Input 2):** Generic cover letter mentioning "your innovative product" and "I'm passionate about". No JD details. No voice match.

**Step 4 score:** Routing 8/20 (description doesn't trigger on "applying"), Specificity 4/20 (no JD reference), Format 12/20 (3-paragraph default), Voice 6/20 (generic), Personalization 4/20. **Total 34/100. Grade C.**

**Step 5 fixes:**
- Description rewrite (Tip 1+2): adds 5 trigger phrases including "applying to", "help me apply", "JD"; adds "Do NOT use for resume rewrites — use /resume-rewrite"
- Step 0 added (Tip 4): table reading the user's `/voice-samples/`, `/resume.md`, and the JD if path provided
- Output format added (Tip 5+10): a worked example of a cover letter the user has approved before
- Domain criterion 4 fix: voice extraction step that reads voice samples and grounds tone

**Step 6 re-run.** Step 7 re-score. **Total 92/100. Grade A+.** Loop ends.

**Step 9 artifact:** all of the above saved to `outputs/improve-skill-runs/cover-letter-writer-2026-05-07/`.

## Before Marking Complete

Do not consider this task finished until all of the following are true:

- [ ] Step 0 context files were read — list which ones in your response (target SKILL.md, twelve-tips.md, audit-checklist.md; examples.md and learnings.md if relevant)
- [ ] Rubric written with 5 criteria, 0–20 anchors, saved to outputs folder
- [ ] At least 3 test inputs generated and saved
- [ ] Original skill ran on every input — outputs saved
- [ ] Original outputs scored against rubric — grades-original.md saved
- [ ] At least one Edit tool call modified the target SKILL.md (unless every original output already scored A+ — then state that explicitly)
- [ ] Improved skill ran on every input — outputs saved
- [ ] Improved outputs scored against rubric — grades-improved.md saved
- [ ] If any improved score under 90, iteration loop ran (up to 3 iterations) and is logged
- [ ] summary.md written: before/after grades, total iterations, what moved
- [ ] One entry appended to `references/learnings.md` in *this* skill's folder

If any item above is unchecked, complete it before finishing.

## After Completing: Log Learning

Append to `references/learnings.md` (in `improve-skill/references/`, not the target skill's folder):

```
Date: [YYYY-MM-DD]
Target: [name and path]
Domain: [PM / job-search / content / personal / engineering / other]
Baseline grade: [X/100]
Final grade: [Y/100]
Iterations: [N]
Highest-leverage fix: [the one edit that moved the score the most]
What didn't work: [edits that didn't move the score, with hypothesis why]
Edge case: [anything unexpected — e.g., skill had no body, multi-skill in one file, references/ pointed at deleted files, target file was 1800 lines]
```

Create the file if it doesn't exist.
