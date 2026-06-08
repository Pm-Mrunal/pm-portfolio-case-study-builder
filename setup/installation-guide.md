# Installation Guide

## Step 1: Install Claude Code

You need Node.js 18+ installed first. Then:

```bash
npm install -g @anthropic-ai/claude-code
```

Verify it works:

```bash
claude --version
```

If you see a version number, you're good. If not, check Node.js is installed: `node --version`

## Step 2: Open the Project

Note: If you downloaded this as a zip from GitHub, make sure the folder name has no spaces before opening it in Claude Code. Rename it to pm-portfolio-case-study-builder if needed — spaces in the path will cause Claude Code to throw a warning on every command.

Navigate to this folder in your terminal:

```bash
cd pm-case-study-builder
claude
```

Claude Code reads `CLAUDE.md` automatically when you open the folder. No pasting required — the system prompt loads on its own.

## Step 3: Try It

Once Claude Code is running, type:

```
/quick-start
```

Then paste any project documents (PRD, notes, anything). You'll get a first draft before you need to fill anything in.

## Recommended Setup

**Cursor** (free, built-in terminal) — cursor.com
Open the `pm-case-study-builder/` folder in Cursor, then use the built-in terminal to run `claude`.

VS Code also works. Or use any standalone terminal app.

## Live Portfolio Review (needed for `/review-portfolio`)

The `/review-portfolio` skill visits your published portfolio in a real browser and critiques it as a recruiter would. The Playwright MCP server it uses is already declared in this repo's `.mcp.json`, so you do not need to add it manually. You only have to approve it once and install the browser:

1. **Approve the server.** The first time you run `claude` in this folder, Claude Code detects the bundled Playwright server and asks you to approve it (project MCP servers always require explicit approval). Approve it. To check the status anytime:

   ```bash
   claude mcp list   # playwright should move from "Pending approval" to "✓ Connected"
   ```

2. **Install the browser** it drives (one time):

   ```bash
   npx playwright install chromium
   ```

3. **Restart Claude Code.** MCP tools load only at startup, so fully quit and reopen Claude Code in this folder after approving. After that, `/review-portfolio [your live URL]` works in any session.

You do not need this to build case studies or generate the portfolio prompt — only to review a deployed site.

## Connecting to a Claude.ai Project (Optional)

For persistent context across sessions, connect this folder to a Claude.ai Project:
1. Create a project at claude.ai
2. Upload your `context-library/` files as project knowledge
3. Continue working in Claude Code — project context will be available

## File Permissions

Make sure Claude Code can read and write to the `outputs/` and `briefings/` folders:

```bash
chmod 755 outputs/
chmod 755 briefings/
```

## Supported Input Formats

| Format | How to Use |
|---|---|
| Plain text | Paste directly into the terminal |
| PDF | Copy-paste the text content, or reference the file path |
| DOCX | Copy-paste content, or reference the file path |
| TXT / Markdown | Paste directly or reference file path |
| Screenshots | Describe the content in text — Claude can't parse images directly in Code |

## Troubleshooting

**"command not found: claude"** — Run `npm install -g @anthropic-ai/claude-code` again. Make sure npm global bin is in your PATH.

**"CLAUDE.md not loading"** — Make sure you `cd` into the `pm-case-study-builder/` folder before running `claude`.

**Output not saving** — Check that the `outputs/` folder exists and is writable. Run `mkdir -p outputs/` if needed.

**`/review-portfolio` says the browser tools aren't available** — The Playwright MCP loads only at startup. Run `claude mcp list` to confirm it shows `✓ Connected`, then fully quit and reopen Claude Code in this folder. If it's missing entirely, run the setup in "Live Portfolio Review" above.
