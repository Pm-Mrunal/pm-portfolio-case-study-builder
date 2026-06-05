---
name: review-as-hiring-manager
description: Launches a hiring-manager sub-agent that does a deep PM-judgment read of a generated case study and returns five-dimension scores, likely interview questions, a verdict, and the top 3 improvements. Use when the user says "review as hiring manager", "does this show PM judgment", "run the hiring manager review", "/review-as-hiring-manager", "is this senior enough", or before a high-priority or senior (Staff/Director/VP) application. Do NOT use for the fast first-pass scan → use /review-as-recruiter instead. Do NOT use to rewrite the case study → use /polish instead. Do NOT use to validate evidence before generating → use /evidence-check instead.
---

# /review-as-hiring-manager — Hiring Manager Lens Review (Sub-Agent)

This skill runs the deep PM-judgment critique as a dedicated sub-agent so the read is done with a fresh, skeptical lens rather than by the same context that wrote the case study.

## Step 0: Resolve What to Review

Determine the case study text to review, in this order:
1. If the user pasted a case study in their message, use that.
2. If the user named a project slug or output file, read `outputs/[that file]`.
3. If neither, list the `.md` files in `outputs/` (excluding `README.md`) and ask: "Which case study should I review? [list]" Then stop and wait.

A hiring manager read needs the full detailed version, not just the TLDR. If only a TLDR is available, tell the user the review will be limited and ask whether to proceed.

Also read `context-library/writing-preferences.md` if it exists and is filled, to capture the target role type and seniority. If it is blank or missing, proceed without it.

## Step 1: Launch the Hiring Manager Sub-Agent

This step is mandatory. Do NOT write the review yourself in the main thread.

Read the full contents of `sub-agents/hiring-manager-reviewer.md`. Then use the **Agent** tool (subagent_type: `general-purpose`) to launch a sub-agent whose prompt is:

- The complete contents of `sub-agents/hiring-manager-reviewer.md` as the role, process, and output-format instructions, followed by
- `--- TARGET ROLE TYPE ---` and the target role and seniority from writing-preferences (or "Not specified — judge against a mid-to-senior PM role." if absent), followed by
- `--- CASE STUDY TO REVIEW ---` and the full case study text resolved in Step 0.

Instruct the sub-agent to follow the Output Format in `sub-agents/hiring-manager-reviewer.md` exactly and to return only that filled-in review.

## Step 2: Relay the Review

Return the sub-agent's review to the user exactly as produced, under a short header naming the case study reviewed. Do not soften the verdict, edit the scores, or drop the interview questions — the questions are a service to the user for interview prep.

Then offer the next step: "Want me to address the Top 3 improvements? I can run /polish, or re-generate the sections that scored lowest."

## Common Shortcuts — Do Not Take These

| What you might think | Why it's wrong |
|---|---|
| "I can just write the hiring manager review inline — it's faster" | The point of this skill is an independent lens. Reusing the authoring context defeats the review. Always launch the sub-agent. |
| "The TLDR is enough to judge PM judgment" | Decision quality and reflection live in the detailed version. Reviewing only the TLDR produces a shallow verdict. Flag it and ask. |
| "I'll skip the interview questions to keep it short" | The likely-interview-questions section is the highest-value output for the user. It is required, not optional. |
