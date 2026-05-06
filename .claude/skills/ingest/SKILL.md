---
name: ingest
description: Parse and chunk project documents. Accepts pasted text, PDF content, DOCX, or TXT. Classifies each document by type and prepares raw material for field-level extraction.
---

# /ingest — Document Ingestion

## When to Use

- User uploads or pastes project documents
- User says "here are my docs" or "I have a PRD to paste"
- Starting a new case study from scratch
- Auto-triggered as Step 1 of the full pipeline

## Inputs

- Pasted text (any format)
- Multiple documents separated by `---`
- File path references for PDFs, DOCX, TXT in the project folder

## Process

### Step 1: Receive Content

Accept all pasted content. Treat `---` separators as document boundaries. If no separator exists, treat everything as one document.

### Step 2: Classify Each Document

Classify each document or section as:
- `prd` — product requirements document, product brief
- `research` — user research, interview notes, synthesis
- `metrics` — data, analytics, funnel reports, dashboards
- `strategy` — strategy docs, decision memos, 1-pagers, roadmaps
- `resume` — resume bullets, LinkedIn experience entries
- `notes` — personal notes, Slack threads, raw observations, anything else

### Step 3: Extract Raw Text

Pull all readable content. Do NOT summarize at this stage. Preserve:
- Specific numbers and percentages
- Names of people, systems, teams, companies
- Dates and timelines
- Direct quotes and verbatim language
- Bullet structures and lists

Chunk long documents into ~500-1,000 token segments. Store metadata per chunk:
```
chunk_id: [auto]
document_type: [classified type]
source_label: [user-provided name or "pasted text"]
text: [raw extracted content]
```

### Step 4: Derive Project Slug

Extract the project name from the ingested content. Convert it to a slug: lowercase, words separated by hyphens, no special characters (e.g., "Checkout Redesign 2024" → `checkout-redesign-2024`).

If no clear project name is found, ask: "What's the name of this project? I'll use it to name the input file."

The project input file for this case study will be: `context-library/project-inputs-[slug].md`

**Never overwrite an existing project input file.** If `context-library/project-inputs-[slug].md` already exists, tell the user: "A file for '[slug]' already exists. Would you like to overwrite it, or use a different name for this project?"

### Step 5: Confirm What Was Ingested

Tell the user:
- Number of documents/chunks processed
- Document types detected
- Any documents that were too short or ambiguous
- The project slug that will be used

Example: "Ingested 3 documents: 1 PRD, 1 research synthesis, 1 set of resume bullets. Project slug: `checkout-redesign`. Input will save to `context-library/project-inputs-checkout-redesign.md`. Continuing to `/extract`..."

### Step 6: Flag Issues

If documents appear to cover more than one project, flag it: "These documents seem to cover multiple projects. Which project should I build the case study around?"

If total content is under 200 words: "This is a short input. The more context you provide, the stronger your case study. Can you add more — rough notes, bullet points, even a verbal description of what you built?"

### Step 7: Auto-Proceed

If the user said "build my case study" or is running `/quick-start`, automatically proceed to `/extract` without asking.

## Notes

- Do not summarize at this stage. Raw detail is preserved for extraction.
- Do not evaluate quality of the content yet — just ingest and classify.
- If a file path is referenced and the file isn't found, tell the user which file is missing before proceeding.
