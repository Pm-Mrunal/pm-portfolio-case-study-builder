# First Session Checklist

## Before You Start

- [ ] Claude Code installed (`npm install -g @anthropic-ai/claude-code`)
- [ ] You're inside the `pm-case-study-builder/` folder in your terminal
- [ ] You have documents for ONE project ready to paste (PRD, notes, metrics, anything)
- [ ] You've read `START-HERE.md`

---

## Session 1: Your First Case Study (20-30 min)

### Step 1: Open Claude Code (2 min)
```bash
cd pm-case-study-builder
claude
```

### Step 2: Gather Your Source Documents (5 min)

Collect any documents about ONE project. You don't need everything — rough notes work.

Strong source documents:
- PRD or product brief
- Research synthesis or interview notes
- Metrics report or dashboard data
- Strategy doc or 1-pager
- Your own resume bullets for this project
- Slack threads or launch notes (copy-paste)
- Any personal notes you took during the project

Weak source documents (still useful, just generates more questions):
- Just a job title and company name
- Vague summary bullets with no specifics
- Team-level outcomes with unclear personal ownership

### Step 3: Run /quick-start (10-15 min)
Type `/quick-start` and paste everything. Claude will:
1. Parse and classify your documents
2. Extract structured data field-by-field
3. Ask up to 5 targeted questions about gaps
4. Generate the recruiter TLDR + detailed version
5. Save to `outputs/`

### Step 4: Answer the Gap Questions Well (5 min)

Claude will ask up to 5 questions about the most important missing details. Good answers make a dramatic difference.

**Strong answers:**
- "I personally owned the discovery phase, roadmap, and stakeholder comms. Engineering and design were separate leads."
- "We saw 23% week-1 retention vs 18% baseline. No direct revenue measurement."
- "The main tradeoff was build vs. buy for the AI layer — we chose build to keep user data in-house."

**Weak answers (avoid):**
- "It went well"
- "I led the whole thing"
- "We had good results"

### Step 5: Review and Polish (5 min)

Read both versions. Run `/polish` if you want a final quality pass. Then optionally:
- `/review-as-recruiter` — would this pass a 6-second scan?
- `/review-as-hiring-manager` — does this show PM judgment?

---

## After Your First Session

Fill in `context-library/writing-preferences.md` so future case studies match your targeting. See `setup/example-writing-preferences.md` for a filled example.

If you asked for any edits to the generated output during this session, run `/learn`. The system logged every change you requested — `/learn` synthesizes those into named style rules sorted by confidence, with evidence cited. Approve the ones that fit and they apply automatically to all future case studies.

For subsequent case studies, start from Step 2 with a new project. Each one takes 15-20 minutes once you're set up.
