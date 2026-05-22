---
name: learn
description: Synthesizes the user's editing history into named style rules tiered by confidence, presents findings for approval, and writes approved rules into writing-preferences.md. Use when the user says "run /learn", "synthesize my edits", "figure out my style preferences", "update my writing preferences from my edits", "what patterns have you picked up from my feedback", "analyze my editing behavior", or after multiple editing sessions on generated outputs. Do NOT use to manually add a new preference → edit context-library/writing-preferences.md directly instead. Do NOT use to generate, polish, or review a case study → use /generate, /polish, or /evidence-check instead.
---

# /learn — Style Learning and Preference Synthesis

## Step 0: Load Context

Read `references/learnings.md` if it exists. Apply patterns flagged as successful. Avoid patterns flagged as failures.

| Source | Path | What to extract |
|--------|------|-----------------|
| Feedback log | `briefings/feedback-log.md` | All entries, total count, date range |
| Writing preferences | `context-library/writing-preferences.md` | Existing stated rules (for conflict detection only) |
| Synthesis prompt | `prompts/learn-prompt.md` | Analysis process (Steps A–E: parse, group, conflict-detect, draft, identify low-confidence signals) |

## Step 1: Validate Input

Check whether `briefings/feedback-log.md` exists and contains at least one entry (look for any line beginning with `## `).

**If the file does not exist or contains no entries:**
Say exactly:
"No feedback has been logged yet. Feedback entries are written automatically whenever you ask for edits to generated outputs. Run `/quick-start` or `/generate`, ask for changes during the session, and the system will log what it learns. Run `/learn` again after at least one editing session."
Stop here.

**If entries exist:** count the total number of entries and the date range (earliest to latest entry date). Continue.

## Step 2: Load Source Files

Read both files in full:
1. `briefings/feedback-log.md` — all entries
2. `context-library/writing-preferences.md` — if it exists

Say: "Reading [N] feedback entries from [earliest date] to [latest date]. Analyzing patterns..."

## Step 3: Run Synthesis

Run the analysis process from `prompts/learn-prompt.md` (Steps A–E: parse and classify entries, group by pattern, detect conflicts, draft proposed text, identify low-confidence signals).

**Output format:** Use the Step 4 format below — not the output format section in `prompts/learn-prompt.md`. The analysis process from learn-prompt.md governs HOW to synthesize; Step 4 below governs HOW to present. They are different. Do not combine them.

Do not present findings until the full synthesis is complete.

## Before Presenting Findings (gate — run before Step 4)

Complete all items before proceeding to Step 4. Do not skip:

- [ ] Every proposed rule is supported by at least one cited log entry — no rules invented
- [ ] `explicit_correction` and `preference_stated` entries qualify as HIGH confidence on a single instance
- [ ] `structural_edit` and `tone_edit` entries require 2 instances for MEDIUM, 3 for HIGH
- [ ] `positive_signal` entries contribute to pattern weight but never qualify a rule alone
- [ ] No rule upgrades "contributed to" ownership language or inflates role claims
- [ ] Conflicted rules are flagged and excluded from 'approve all'
- [ ] Proposed text for each rule is specific enough to change generation output — not a vague preference
- [ ] Only the "Learned Style Rules" section of `writing-preferences.md` will be written to

## Step 4: Present Findings

Output findings in this exact structure. Do not deviate from the format.

```
=== WHAT I'VE LEARNED FROM YOUR FEEDBACK ===

[N] feedback entries analyzed · [date range]

── HIGH CONFIDENCE ─────────────────────────────────────────
Supported by 3+ entries, or by a single explicit correction or stated preference.
Recommended to approve without review.

[number]. [RULE NAME IN CAPS]
What this means: [one sentence explaining the rule in plain language]
Evidence: [2-3 brief entry summaries that support this rule — include date and project slug]
Proposed addition to writing-preferences.md:
  "[exact text to add, ready to paste]"

[repeat for each high-confidence rule]

── MEDIUM CONFIDENCE ────────────────────────────────────────
Supported by 2 entries of the same type. Review before approving.

[number]. [RULE NAME IN CAPS]
What this means: [one sentence]
Evidence: [entry summaries with date and project slug]
Proposed addition:
  "[exact text]"

[repeat for each medium-confidence rule]

── LOW CONFIDENCE — FOR AWARENESS ONLY ─────────────────────
Single signals. Not proposed for approval. Shown so you can decide whether to state
them explicitly in your writing preferences.

· [brief description of the signal, date, and project slug]

[repeat for each low-confidence signal]

── CONFLICTS DETECTED ───────────────────────────────────────
[Only show this block if conflicts exist. Omit entirely if none.]

Rule [N] conflicts with your existing preference:
  Existing: "[existing preference text verbatim]"
  Proposed: "[proposed rule text]"
These will not be applied unless you explicitly approve them by number.

─────────────────────────────────────────────────────────────
[N] rules ready to apply ([N] high, [N] medium).

Type 'approve all' to apply all high and medium confidence rules.
Type numbers to select specific rules (e.g. '1 3').
Type a number followed by your edit to modify a rule before approving (e.g. '2: always lead outcomes with the top metric, not a percentage').
Type 'skip' to exit without changes.
```

## Step 5: Handle User Response

Wait for the user's input before proceeding. Then:

**'approve all'**
Apply all high and medium confidence rules. Skip low confidence signals. Skip conflicted rules unless the user explicitly listed their numbers.

**Number list (e.g. '1 3 5')**
Apply only those rules. Skip all others.

**Modified rule (e.g. '2: [new text]')**
Use the user's version verbatim for that rule number. Apply it as written.

**'skip'**
Write nothing. Say: "No changes made. Your feedback log is preserved — run /learn any time to revisit these findings."

**Conflicted rules**
Apply only if the user listed the number explicitly. If approved, the new rule supersedes the old one. Note this in the Learned Style Rules section.

## Step 6: Write Approved Rules

Open `context-library/writing-preferences.md`.

**If the file does not exist:** create it with only the Learned Style Rules section below.

**If a `## Learned Style Rules` section already exists:** append new rules inside it. Do not create a duplicate section.

**If the section does not exist:** append the following block at the end of the file, after a blank line:

```markdown

---

## Learned Style Rules

> Generated by /learn. Based on observed editing behavior, approved by you.
> Each rule below was inferred from feedback entries and confirmed.
> Edit or delete any rule that no longer reflects your preferences.
> Last updated: [YYYY-MM-DD] — [N] entries synthesized, [N] rules applied.

[approved rules, one per bullet:]
- [RULE NAME]: [rule description in one sentence]
```

**If the section exists and you are adding to it:** update the "Last updated" line and append new rules to the bullet list. Do not touch any other line in the file.

Do not modify the Tone, Target Roles, Weaknesses, Strengths, or Notes sections.

## Step 7: Mark Log Entries as Processed

Prepend the following line to the top of `briefings/feedback-log.md` (below the file header, above the first entry):

```
> Synthesized [YYYY-MM-DD]: [N] entries processed · [N] rules approved · [N] rules skipped.
```

If a prior synthesis line already exists, replace it — do not accumulate multiple synthesis lines at the top.

## Step 8: Confirm and Close

Tell the user:

"[N] rules added to your writing preferences. They will be applied automatically in all future case studies.

Next steps:
- Run `/quick-start` on a new project to see the rules in action
- Run `/learn` again after more sessions — confidence increases with more entries
- Edit `context-library/writing-preferences.md` directly to modify or remove any rule"

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|----------------------|----------------|
| "The user just wants to add a preference — I can skip synthesis and write to writing-preferences.md directly" | /learn only writes rules that are grounded in feedback log entries. For manual additions, tell the user to edit the file directly. |
| "The feedback log has only 2 entries — I'll skip tiering and just list the rules" | Confidence tiers are the whole point of /learn. Apply them regardless of entry count. If <3 entries and no explicit corrections, add the "low entry count" note but still tier correctly. |
| "Both output formats look similar — I'll use the learn-prompt.md format since it's more detailed" | Use the Step 4 format exactly. learn-prompt.md's output format section is superseded by Step 4. Never mix them. |
| "The gate items look fine — I'll present findings and check them after if needed" | The gate in "Before Presenting Findings" runs BEFORE Step 4. Every item must be true before any output is shown. |
| "A positive_signal entry strongly supports this rule — I'll give it MEDIUM confidence" | positive_signal entries are supporting weight only. They never qualify a rule alone at any tier. |

## Before Marking Complete

Do not consider this task finished until all of the following are true:

- [ ] `briefings/feedback-log.md` read and entry count confirmed
- [ ] All synthesis steps (A–E from learn-prompt.md) completed before presenting
- [ ] Pre-presentation gate passed (all 8 items checked)
- [ ] Findings presented in Step 4 format with numbered rules
- [ ] User response received before writing anything to writing-preferences.md
- [ ] Only approved rules written — not skipped, conflicted, or low-confidence rules
- [ ] "Learned Style Rules" section updated (not overwritten); "Last updated" line refreshed
- [ ] Synthesis annotation prepended to `briefings/feedback-log.md`
- [ ] Step 8 close message sent

## Out of Scope

This skill does NOT handle:
- Manually adding a new preference to writing-preferences.md → edit the file directly
- Generating, drafting, or refining case study content → use `/generate` or `/polish`
- Validating evidence strength → use `/evidence-check`
- Ingesting raw project documents → use `/ingest`
- Running the full pipeline on a new project → use `/quick-start`

## After Completing: Log Learning

Append one entry to `references/learnings.md` (create if it doesn't exist):

```
Date: [today]
Entry count: [N]
Rules approved: [N]
What worked: [specific pattern that produced clear, actionable rules]
What didn't: [any synthesis failure — e.g., conflicting signals, insufficient entries, ambiguous inferred preferences]
Edge case: [anything unexpected — empty log, all positive_signals, bulk approval, conflict detected]
```
