---
name: learn
description: Synthesizes the user's editing history into named style rules, presents findings by confidence tier, and writes approved rules into writing-preferences.md. Turns implicit feedback into explicit, persistent preferences.
---

# /learn — Style Learning and Preference Synthesis

## When to Use

- After any session where the user asked for edits to generated outputs
- When the user wants to understand what patterns Claude has inferred from their behavior
- When the user wants to update `writing-preferences.md` based on observed editing behavior rather than manual introspection
- Periodically — the more sessions logged, the higher the confidence of inferred rules

## What This Skill Does

Reads the accumulated feedback log at `briefings/feedback-log.md`, groups entries by inferred preference pattern, assigns confidence based on signal type and frequency, and surfaces named style rules to the user. The user approves rules individually or in bulk. Approved rules are appended to `context-library/writing-preferences.md` under a dedicated "Learned Style Rules" section that all skills auto-load.

This skill does not modify any prompt files or system behavior — it updates only the user's personal preferences file.

## Inputs

- `briefings/feedback-log.md` — accumulated feedback entries (primary source; required)
- `context-library/writing-preferences.md` — current stated preferences (for conflict detection; optional)
- `prompts/learn-prompt.md` — synthesis prompt

## Process

### Step 1: Validate Input

Check whether `briefings/feedback-log.md` exists and contains at least one entry (look for any line beginning with `## `).

**If the file does not exist or contains no entries:**
Say exactly:
"No feedback has been logged yet. Feedback entries are written automatically whenever you ask for edits to generated outputs. Run `/quick-start` or `/generate`, ask for changes during the session, and the system will log what it learns. Run `/learn` again after at least one editing session."
Stop here.

**If entries exist:** count the total number of entries and the date range (earliest to latest entry date). Continue.

---

### Step 2: Load Source Files

Read both files in full:
1. `briefings/feedback-log.md` — all entries
2. `context-library/writing-preferences.md` — if it exists

Say: "Reading [N] feedback entries from [earliest date] to [latest date]. Analyzing patterns..."

---

### Step 3: Run Synthesis

Run `prompts/learn-prompt.md` against the loaded content.

The synthesis must produce, in order:
1. A list of named style rules, each with: rule name, supporting evidence entries, confidence tier, and exact proposed text for `writing-preferences.md`
2. A separate list of low-confidence signals (single non-correction instances) — shown for awareness only, not proposed for approval
3. A conflict list — any proposed rule whose meaning contradicts an existing explicitly stated preference in `writing-preferences.md`

Do not present findings until the full synthesis is complete.

---

### Step 4: Present Findings

Output findings in this exact structure. Do not deviate from the format.

```
=== WHAT I'VE LEARNED FROM YOUR FEEDBACK ===

[N] feedback entries analyzed · [date range]

── HIGH CONFIDENCE ─────────────────────────────────────────
Supported by 3+ entries, or by a single explicit correction or stated preference.
Recommended to approve without review.

[number]. [RULE NAME IN CAPS]
What this means: [one sentence explaining the rule in plain language]
Evidence: [2-3 brief entry summaries that support this rule]
Proposed addition to writing-preferences.md:
  "[exact text to add, ready to paste]"

[repeat for each high-confidence rule]

── MEDIUM CONFIDENCE ────────────────────────────────────────
Supported by 2 entries of the same type. Review before approving.

[number]. [RULE NAME IN CAPS]
What this means: [one sentence]
Evidence: [entry summaries]
Proposed addition:
  "[exact text]"

[repeat for each medium-confidence rule]

── LOW CONFIDENCE — FOR AWARENESS ONLY ─────────────────────
Single signals. Not proposed for approval. Shown so you can decide whether to state
them explicitly in your writing preferences.

· [brief description of the signal and which entry it came from]

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

---

### Step 5: Handle User Response

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

---

### Step 6: Write Approved Rules

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

---

### Step 7: Mark Log Entries as Processed

Prepend the following line to the top of `briefings/feedback-log.md` (below the file header, above the first entry):

```
> Synthesized [YYYY-MM-DD]: [N] entries processed · [N] rules approved · [N] rules skipped.
```

If a prior synthesis line already exists, replace it — do not accumulate multiple synthesis lines at the top.

---

### Step 8: Confirm and Close

Tell the user:

"[N] rules added to your writing preferences. They will be applied automatically in all future case studies.

Next steps:
- Run `/quick-start` on a new project to see the rules in action
- Run `/learn` again after more sessions — confidence increases with more entries
- Edit `context-library/writing-preferences.md` directly to modify or remove any rule"

---

## Quality Checks (run before Step 4)

- [ ] Every proposed rule is supported by at least one log entry — no rules invented
- [ ] `explicit_correction` and `preference_stated` entries qualify as HIGH confidence on a single instance
- [ ] `structural_edit` and `tone_edit` entries require 2 instances for MEDIUM, 3 for HIGH
- [ ] `positive_signal` entries contribute to pattern weight but never qualify a rule alone
- [ ] No rule upgrades "contributed to" ownership language or inflates role claims
- [ ] Conflicted rules are flagged and excluded from 'approve all'
- [ ] The proposed text for each rule is specific enough to affect generation — not a vague preference
- [ ] Only the "Learned Style Rules" section of `writing-preferences.md` is written to
- [ ] The feedback log is never deleted, truncated, or modified beyond the synthesis annotation
