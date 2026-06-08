---
name: interview
description: Builds a case study from a guided interview when the user has no documents to paste — only the project in their head. Conducts a relentless, one-question-at-a-time discovery session, checkpointing every answer into the project schema, until it has captured everything a strong document set would provide. Use when the user says "interview me", "grill me", "I don't have docs", "it's all in my head", "I don't have anything written down", "ask me questions", "I'd rather answer questions", or picks the answer-questions route inside /quick-start. Do NOT use when the user has documents to paste → use /quick-start. Do NOT use to fill 3-5 gaps in an already-extracted project → use /gap-detect. Do NOT use to write the case study itself → use /generate.
---

# /interview — Build a Case Study From Your Head, Not Your Docs

This skill replaces the document-paste front door. The user has a real project in their head but nothing written down. Your job is to extract what is in their head into the same structured project schema that `/extract` produces from documents, so the rest of the pipeline (evidence gate → `/generate` → `/visual-layer` → `/polish`) runs unchanged.

The interview, not your memory, is the source of truth. You checkpoint every answer to disk before asking the next question.

## Critical — Read First

- **Never invent, suggest, or lead on facts.** For factual fields — metrics, outcomes, dates, user quotes, revenue, adoption — ask open questions only. Never offer a best-guess number ("a 30% lift?") for the user to rubber-stamp. A suggested fact the user merely confirms is a fabricated fact. This violates the project Hard Rules. Best-guess phrasing is allowed ONLY for framing and structure questions (e.g. "sounds like the core tension was speed vs. accuracy — is that right?"), never for numbers, quotes, or outcomes.
- **A verbal answer is valid evidence.** When the user states something, capture it with `confidence: high` and `evidence: "stated in interview [date]"`. This is primary-source material, not invention.
- **Default to contribution, not ownership.** If the user says "we" or is vague about who did what, capture it as "contributed to" and ask one clarifying question. Never upgrade to "I led" / "I owned" unless the user states it explicitly.
- **The 5-question cap does NOT apply here.** That cap governs `/gap-detect` follow-ups after documents exist. This interview IS the source material, so it runs long by design — target ~15 questions, and keep going past 15 if any major section is still thin, until the user tells you to stop.
- **Tell the user they are in control.** State up front that they can stop you at any time by saying "stop" or "that's enough", and that you will then play everything back for review.

## Step 0: Read Before You Start

Read these before asking the first question. Do not interview blind.

| Source | Path | What to extract |
|--------|------|-----------------|
| Schema field list | `prompts/extraction-prompts.md` | The eight sections and their exact fields — this is your question backbone |
| Writing preferences | `context-library/writing-preferences.md` | Tone, targeting, emphasis — apply if filled; skip silently if blank |
| Existing project slugs | `outputs/` and `context-library/project-inputs-*.md` | Warn the user if the project name they give already has a file |
| Experience library | `context-library/experience-library.md` | Cross-project context that may answer a question without asking |
| Past learnings | `references/learnings.md` | Interview patterns that worked or failed before — apply / avoid |

If a question can be answered from these files, do not ask it. Surface only what is net-new.

## The Schema — Track These As You Go

You are filling these eight sections (from `prompts/extraction-prompts.md`). Mark each as you capture it. Do not end the interview while a high-priority section (1–6) is still empty unless the user has told you to stop.

```
[ ] 1. Problem          — who, prior state, why it mattered, 3-4 distinct dimensions
[ ] 2. Role & ownership — owned vs contributed vs others, partners, team composition
[ ] 3. Discovery        — research methods, top insights, assumptions that changed
[ ] 4. Strategy         — options considered, selected direction, tradeoffs, rationale
[ ] 5. Solution         — what was built, key changes, scope boundaries, rollout
[ ] 6. Outcomes         — launch status, quant results, qual results, validation signals
[ ] 7. Reflection       — learnings, what you'd do differently, open questions
[ ] 8. Pull quotes      — verbatim user/customer lines or memorable PM observations
```

## Step 1: Open the Session

Say this, in one short message:

"No docs needed. I'll interview you one question at a time and turn your answers into the same structured material a strong document set would give us, then run it through the case study pipeline. This usually takes about 15 questions. You can stop me anytime by saying 'stop' or 'that's enough' — when you do, I'll play everything back for your review before we generate. Let's start."

Then ask Question 1: the project name, your role/title on it, and a one-line description of what it was.

From the project name, derive a slug (lowercase, hyphen-separated). If `context-library/project-inputs-[slug].md` already exists, ask whether to use a different name before continuing.

## Step 2: Create the Capture File

Immediately after Q1, create `context-library/project-inputs-[slug].md` in the schema format `/extract` produces. Each field carries `value`, `confidence`, `evidence`, `needs_user_input`. Seed it with the project identity from Q1 and a header noting it was built by interview on [date].

This file is the deliverable of the interview. Create it before asking Q2.

## Step 3: Interview — One Question at a Time

Walk the eight sections in order (1 → 8). Upstream before downstream: settle the problem before discussing the solution to it.

For each question:
1. Ask exactly one question. Keep it concrete and short.
2. For framing/structure questions, you may offer a recommended read of their answer to confirm or correct ("sounds like X — right?"). For factual fields (metrics, outcomes, dates, quotes), ask open — never suggest the value.
3. Wait for the answer.
4. **Checkpoint before the next question** (Step 4).
5. If the user cannot answer, capture it as a flag (`needs_user_input: true`) with a note, and move on. Do not stall.

Coverage targets per section:
- **Problem:** who was affected, the prior state, why it mattered to the business or users, and 3-4 distinct nameable dimensions of the problem.
- **Role & ownership:** what they personally owned vs. contributed to vs. what others owned; cross-functional partners; team composition. Probe any "we" for the personal slice.
- **Discovery:** methods (what they did) separately from insights (what they learned); which assumption changed because of an insight.
- **Strategy:** at least one decision where real alternatives were considered and rejected, with the tradeoff they accepted and why.
- **Solution:** what specifically was built, what was deliberately out of scope, and the rollout/launch approach.
- **Outcomes:** launch status (shipped / pilot / pre-launch / prototype), then any quantitative results, qualitative results, and validation signals — asked open. If there are no hard metrics, capture that honestly; do not manufacture one.
- **Reflection:** a learning specific to this project (not a generic PM lesson), one thing they would do differently, and a real open question.
- **Pull quotes:** any verbatim line from a user, customer, or teammate, or a memorable thing they themselves said about the project.

## Step 4: The Checkpoint Rule (Non-Negotiable)

After EVERY answer, before the next question:
- Write the captured facts into the matching schema fields in `context-library/project-inputs-[slug].md`, in the user's words where the wording matters.
- Set `confidence: high`, `evidence: "stated in interview [date]"`. Set `needs_user_input: true` for anything they flagged.
- Correct earlier fields if a later answer changes them.
- Only then ask the next question.

Never batch answers. One answer, one write. If the session is interrupted at any point, the file already holds everything said so far.

## Step 5: Knowing When to Stop

Stop when EITHER:
- The user says "stop", "that's enough", or similar — honor it immediately, even mid-section.
- You have covered all eight sections with no high-priority section (1–6) empty, AND you have reached roughly 15 questions.

If you hit ~15 questions but a high-priority section is still thin or empty, do not stop on your own. Tell the user which section still needs depth, ask if they want to keep going or wrap up, and continue if they do. The 15 is a target, not a ceiling.

Before stopping on your own, offer a completeness backstop: "Anything about this project we haven't touched that you'd want a hiring manager to see?"

## Step 6: Play It Back for Review

When the interview ends (either trigger), do NOT jump to generation. First:
1. Do a final read of `context-library/project-inputs-[slug].md` for contradictions or gaps; reconcile them.
2. Give the user a structured recap, section by section: what was captured (in 1-2 lines each), what is still flagged as missing, and where the evidence looks thin.
3. Ask: "Does this look right? Correct anything, add anything, or say 'generate' and I'll build the case study."

Apply any corrections to the file, then proceed only on their go-ahead.

## Step 7: Hand Off to the Pipeline

The interview has already done the work of `/ingest`, `/extract`, AND `/gap-detect` (it gap-filled inline by design). Do NOT re-run those.

Rejoin the pipeline at the silent evidence gate:
1. Run the evidence gate (outcomes strength + overclaiming risk). Head-only projects are often thin on hard outcomes, so expect a warning here — surface it in one line and offer `/evidence-check` or generate-now-with-conservative-framing.
2. On the user's go-ahead, run `/generate`, then `/visual-layer`, then `/polish` — same as `/quick-start` from Step 5 onward.

## Output Format

The interview produces a populated `context-library/project-inputs-[slug].md`, then the standard pipeline output. The project-inputs file uses the extraction schema, e.g.:

```markdown
# Project Inputs: checkout-redesign
Source: interview · Date: 2026-06-07

## Problem
- problem_statement: { value: "Guest checkout dropped 40% of carts at the address step", confidence: high, evidence: "stated in interview 2026-06-07", needs_user_input: false }
- who: { value: "First-time mobile buyers", confidence: high, evidence: "stated in interview 2026-06-07" }
- prior_state: { value: "Single 11-field form, no autofill", confidence: high, evidence: "stated in interview 2026-06-07" }
- why_it_mattered: { value: "Checkout was the single largest revenue leak", confidence: high, evidence: "stated in interview 2026-06-07" }

## Role & Ownership
- what_i_owned: { value: ["Problem framing", "Final flow spec"], confidence: high, evidence: "stated in interview 2026-06-07" }
- what_i_influenced: { value: ["Visual design"], confidence: high, evidence: "stated in interview 2026-06-07" }
...
## Outcomes
- launch_status: { value: "pilot", confidence: high, evidence: "stated in interview 2026-06-07" }
- quantitative_results: { value: [], confidence: high, evidence: "user stated no hard metrics yet", needs_user_input: true }
```

## Worked Example

Interview excerpt (factual vs. framing handling):

```
You (framing, recommendation allowed):
  "It sounds like the core tension was launch speed versus data completeness —
   did I read that right?"
User: "Yeah, exactly."
→ Checkpoint: tradeoffs.value = "Accepted thinner data capture to ship the
   pilot in 6 weeks", confidence: high.

You (factual, open only — NO suggested number):
  "What happened to cart completion after the pilot? Any numbers, or is it
   too early?"
User: "Too early, we only have two weeks of data."
→ Checkpoint: quantitative_results.value = [], needs_user_input: true,
   note: "pilot only 2 weeks old, no completion delta yet". You do NOT
   write a percentage.
```

## Out of Scope

This skill does NOT handle:
- A user who has documents to paste → use `/quick-start`
- Filling 3-5 targeted gaps in an already-extracted project → use `/gap-detect`
- Validating evidence strength or finding the best angle → use `/evidence-check`
- Writing the recruiter TLDR or detailed version → use `/generate`
- Building the portfolio website → use `/build-portfolio`

## Cross-Skill Routing

- After playback, if outcomes are thin or ownership risks overclaiming → the evidence gate fires; offer `/evidence-check`.
- On the user's go-ahead → hand to `/generate` → `/visual-layer` → `/polish`.
- If partway through the user reveals they actually have documents → stop and route to `/quick-start`; pasted docs are richer than recalled ones.

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "I'll hold answers in context and write the file once at the end" | The checkpoint rule exists because long interviews lose context. One answer, one write. The file is the source of truth, not your memory. |
| "They seemed unsure on the metric, I'll suggest a plausible one to confirm" | A suggested fact the user rubber-stamps is a fabricated fact. Factual fields are open-question only. Capture "no metric yet" honestly. |
| "I've asked 5 questions, I should stop — that's the cap" | The 5-question cap is for /gap-detect, not the interview. This runs ~15 and longer until sections 1-6 are covered or the user stops you. |
| "They said 'we built it', I'll write 'I led the build'" | Default to "contributed to" on any ambiguous ownership. Upgrade only on an explicit personal claim. |
| "Interview's done, I'll generate immediately" | Always play the captured schema back for review first. The user catches errors and gaps before they harden into a case study. |
| "I'll re-run /gap-detect after to be safe" | The interview already gap-filled inline. Rejoin at the evidence gate, not gap-detect. Re-running it just repeats questions. |

## Before Marking Complete

- [ ] Step 0 files read (list which ones) — schema field list confirmed as the question backbone
- [ ] Capture file `context-library/project-inputs-[slug].md` created right after Q1
- [ ] Every answer checkpointed to the file before the next question (one write per answer)
- [ ] High-priority sections 1-6 captured, OR the user explicitly chose to stop early
- [ ] No suggested numbers, outcomes, or quotes — factual fields filled only from what the user stated
- [ ] Ambiguous ownership captured as "contributed to", not "led"/"owned"
- [ ] Played the schema back for review and applied corrections before generating
- [ ] Evidence gate run before handing to /generate
- [ ] One entry appended to `references/learnings.md`

## After Completing: Log Learning

Append to `references/learnings.md`:
Date / What worked / What didn't / Edge case (e.g. a section that consistently runs thin in head-only interviews, a question that reliably unlocked detail).
