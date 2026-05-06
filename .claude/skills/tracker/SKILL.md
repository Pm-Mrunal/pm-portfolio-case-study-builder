---
name: tracker
description: Log completed case studies and track portfolio coverage. Shows what's been written, what's missing, and what to build next.
---

# /tracker — Portfolio Coverage Tracker

## When to Use

- User says "/tracker" or "show my portfolio status"
- User asks "what case studies do I have?" or "what should I write next?"
- After completing a case study and wanting to log it
- Weekly or monthly portfolio review

## Inputs

- `briefings/portfolio-tracker.md` — the persistent portfolio log (auto-created if it doesn't exist)
- `context-library/writing-preferences.md` — target role type, to assess coverage gaps

## Process

### Step 1: Load or Create the Tracker

Read `briefings/portfolio-tracker.md`. If it doesn't exist, create it with the empty template below and tell the user: "Starting your portfolio tracker. Tell me about the case studies you've already written — or I'll log this session's output automatically."

### Step 2: Log Completed Case Studies

When a case study is completed in this session, automatically add an entry:

```markdown
## [Project Title]
- Company: [company name]
- Stage: [shipped / pilot / pre-launch / internal]
- PM Type: [growth / 0-to-1 / platform / redesign / AI / enterprise / other]
- Outcomes available: [yes — quantitative / yes — directional only / no]
- File: outputs/[filename]
- Generated: [YYYY-MM-DD]
- Status: [draft / polished / reviewed / published]
```

### Step 3: Assess Coverage

Based on the logged portfolio and `context-library/writing-preferences.md`, identify coverage gaps:

**PM skill coverage** — does the portfolio show breadth across:
- [ ] Discovery and user research
- [ ] Strategic decision-making and tradeoffs
- [ ] Metrics-driven outcomes
- [ ] Execution and shipping under constraints
- [ ] 0-to-1 building
- [ ] Iteration and learning from failure

**Stage coverage** — does the portfolio show:
- [ ] At least one shipped product with outcomes
- [ ] At least one complex or ambiguous project
- [ ] Range across company stages or product types (if applicable)

**Audience coverage**:
- [ ] At least one case study optimized for recruiters (fast scan)
- [ ] At least one that would hold up to deep hiring manager scrutiny

### Step 4: Recommend What to Write Next

Based on gaps, recommend the next case study to build:

Example: "Your portfolio has two growth case studies with strong metrics but no 0-to-1 project. If you have a project where you built something from scratch — even an internal tool or a feature that didn't ship — that would balance your portfolio well."

### Step 5: Display Status

Output a clean portfolio status summary:

```
Portfolio Status — [Date]

Completed case studies: [N]
[List each with title, company, type, outcomes status]

Coverage gaps:
- [Gap 1]
- [Gap 2]

Recommended next case study: [What to build and why]
```

## Portfolio Tracker Template

```markdown
# Portfolio Tracker

Last updated: [date]

---

## Case Studies

[entries added here as each is completed]

---

## Coverage Notes

[notes on gaps and recommendations]
```
