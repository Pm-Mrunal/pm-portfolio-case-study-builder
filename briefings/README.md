# Briefings

Session notes, portfolio tracking, and feedback logs are stored here. All files in this folder are excluded from git — they contain your personal working data.

## Files

- `feedback-log.md` — Created automatically whenever you ask for edits to generated outputs. This is the input to `/learn`. Do not edit manually.
- `portfolio-tracker.md` — Created automatically by `/tracker`. Logs all completed case studies and tracks portfolio coverage gaps.
- `session-notes-[date].md` — Optional session notes, created when you ask Claude to save context between sessions.

---

## Feedback Log Format

Each entry in `feedback-log.md` follows this structure:

```markdown
## [YYYY-MM-DD] | [project-slug] | [Section] | [signal_type]

**Request:** [what the user asked for]
**Change:** [what changed in the output]
**Inferred preference:** [the underlying style rule this edit suggests]
**Signal type:** explicit_correction | preference_stated | structural_edit | tone_edit | positive_signal
```

Run `/learn` to synthesize accumulated entries into named style rules and update your writing preferences.

---

This folder is excluded from git by default.
