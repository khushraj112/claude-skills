---
name: daily-slack-update
description: Turn raw Claude Code / AI coding-agent work logs into a short, humanised daily Slack stand-up update. Extracts the work that was actually accomplished, drops commits, branches, file paths and tool noise, and formats it as a NOTION/Other hours header with per-project bullet points. Use when asked for a daily update, stand-up summary, Slack update, or "what did I get done today" from agent output, session transcripts, or a work log.
---

# Daily Slack Update

Convert raw AI-agent work logs into the daily Slack stand-up update. Run two stages in order. Stage 1 decides *what* is true; Stage 2 decides *how it reads*. Do not merge them — formatting pretty prose over an inaccurate extraction is the main failure mode.

## Stage 1 — Work Extractor

Read the raw log and identify the meaningful work that was actually completed.

**Extract:**

- Features implemented
- Bugs fixed
- Research completed
- Testing completed
- Planning completed
- Important investigations
- Meaningful configuration or admin work
- Important project progress

**Ignore:**

- Commit hashes
- Branch names
- PR numbers
- File paths
- Internal implementation details
- Repeated debugging logs
- Tool output
- Git operations, unless the operation itself is the meaningful work
- Technical details that do not help explain what was accomplished

**Accuracy rules — these matter more than the wording:**

- Do not claim something was completed if the source only says it was researched, planned, or discussed.
- Keep research, planning, implementation, and testing distinct. A day spent investigating an approach is investigation, not delivery.
- If work was started but not finished, say so plainly ("Started on…", "Looked into…") rather than implying it shipped.
- If the log is ambiguous about whether something landed, prefer the weaker claim.
- Never invent work that is not in the source.

**Group by project.** One log often spans several projects. Attribute each piece of work to the project it belongs to before formatting. Many small commits touching the same area are one outcome, not five bullets — merge them.

## Stage 2 — Slack Formatter

Turn the extracted work into the update.

### Structure

```
**NOTION: Xh | Other: Xh**
**Project Name:(Xh) (Xh in Notion)**

* Humanised task
* Humanised task
* Humanised task
```

For multiple projects, repeat the project heading block for each one. The `NOTION: Xh | Other: Xh` header appears once, at the top.

### Hours

Hours are usually not present in an agent log. Use hours stated in the source or given by the user. If they are missing, leave the `Xh` placeholders in place and tell the user which numbers to fill in — do not estimate or invent hours.

### Writing style

Short, natural, clear, outcome-focused. It should read like a person typing a stand-up note, not a changelog.

Use natural openers: Fixed…, Worked on…, Updated…, Improved…, Tested…, Researched…, Planned…, Investigated…

**Do not:**

- Put square brackets around task points
- Add extra headings inside a project
- Include commit IDs or branch names
- Mention implementation details unless they materially explain the work
- Make a small technical fix sound like a major feature
- Exaggerate, pad, or add filler

Aim for a handful of bullets per project. If there are more than about five, the extraction was too granular — go back and merge.

The question being answered is:

> What did I actually get done today?

Not:

> What files, commits, branches, or technical operations did I perform?

## Example

Raw agent output containing:

> "Commit 3da5258 updated search logic, changed templates/search.json, fixed Shopify fuzzy matching, then commit 8035ba5 fixed three-column layout…"

Becomes:

* Improved the search page to properly handle exact matches, fuzzy results, and empty states.
* Fixed the search grid layout and product card display.
* Tested the updated search experience across desktop and mobile.

Note what happened: two commits and a template path collapsed into outcomes, the hashes and filenames disappeared, and testing is listed separately because it was genuinely done.

## Before returning the update

- Every bullet traces back to something real in the source.
- Research and planning are not described as shipped work.
- No commit hashes, branch names, PR numbers, or file paths remain.
- No square brackets around bullets.
- It reads like a person wrote it.
