---
name: review-as-recruiter
description: Launches a recruiter-lens sub-agent that does a 6-second-scan critique of a generated PM case study and returns scores, flagged issues, a pass verdict, and the top 3 fixes. Use when the user says "review as recruiter", "would a recruiter pass this", "run the recruiter review", "/review-as-recruiter", "is this recruiter-ready", or after /generate or /polish when they want an outside read before sharing. Do NOT use to rewrite or edit the case study → use /polish instead. Do NOT use to validate evidence strength before generating → use /evidence-check instead. Do NOT use for the deep PM-judgment read → use /review-as-hiring-manager instead.
---

# /review-as-recruiter — Recruiter Lens Review (Sub-Agent)

This skill runs the recruiter critique as a dedicated sub-agent so the review is done with a fresh, skeptical lens rather than by the same context that wrote the case study.

## Step 0: Resolve What to Review

Determine the case study text to review, in this order:
1. If the user pasted a case study in their message, use that.
2. If the user named a project slug or output file, read `outputs/[that file]`.
3. If neither, list the `.md` files in `outputs/` (excluding `README.md`) and ask: "Which case study should I review? [list]" Then stop and wait.

Also read `context-library/writing-preferences.md` if it exists and is filled, to capture the target role type. If it is blank or missing, proceed without it.

## Step 1: Launch the Recruiter Sub-Agent

This step is mandatory. Do NOT write the review yourself in the main thread.

Read the full contents of `sub-agents/recruiter-reviewer.md`. Then use the **Agent** tool (subagent_type: `general-purpose`) to launch a sub-agent whose prompt is:

- The complete contents of `sub-agents/recruiter-reviewer.md` as the role, process, and output-format instructions, followed by
- `--- TARGET ROLE TYPE ---` and the target role from writing-preferences (or "Not specified — judge against a generalist PM role." if absent), followed by
- `--- CASE STUDY TO REVIEW ---` and the full case study text resolved in Step 0.

Instruct the sub-agent to follow the Output Format in `sub-agents/recruiter-reviewer.md` exactly and to return only that filled-in review.

## Step 2: Relay the Review

Return the sub-agent's review to the user exactly as produced, under a short header naming the case study reviewed. Do not soften the verdict or edit the scores.

Then offer the next step: "Want me to apply the Top 3 fixes? I can run /polish on this case study, or re-generate the weak sections."

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "I can just write the recruiter review inline — it's faster" | The point of this skill is an independent lens. Reusing the authoring context defeats the review. Always launch the sub-agent. |
| "No output file named, so I'll review the most recent one" | Different projects have different target roles. Ask which case study unless it is unambiguous. |
| "writing-preferences is blank, so I'll skip the role check" | Fit Signal needs a target role. If none exists, tell the sub-agent to judge against a generalist PM role rather than omitting it. |
