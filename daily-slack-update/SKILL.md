---
name: daily-slack-update
description: Turn raw Claude Code / AI coding-agent work logs into two outputs — a detailed Notion work record and a condensed Slack stand-up update. Notion is the source of record with full context, testing and outcomes; Slack is a short summary derived from it. Extracts what was actually accomplished and drops commits, branches, file paths and tool noise. Use when asked for a daily update, stand-up summary, Notion work log, Slack update, or "what did I get done today" from agent output, session transcripts, or a work log.
---

# Daily Work → Notion + Slack

This is not a Slack formatter. It is a daily work system with two outputs at two different altitudes.

**Notion is the source of record.** It captures the day's work in enough detail to be useful weeks later — implementation decisions, testing, investigation, outcomes, hours.

**Slack is the communication layer.** It is a quick stand-up read containing only the most meaningful accomplishments, with links back to Notion for anyone who wants the detail.

```
                    RAW AI OUTPUT
                         │
                         ▼
                 Extract real work
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
        NOTION VERSION          SLACK VERSION
          Detailed               Minimized
          5–10 points             3–6 points
          Full context            Key outcomes
          Hours                   Notion links
          Future reference        Stand-up ready
```

Run the three stages in order. Never skip Stage 2 and write Slack directly from the raw log — Slack is derived from the Notion record, not from the noise.

## Stage 1 — Extract real work

Read the raw log and identify what was genuinely accomplished.

**Extract:** features implemented, bugs fixed, research completed, testing completed, planning completed, important investigations, meaningful configuration or admin work, important project progress.

**Ignore:** commit hashes, branch names, PR numbers, file paths, internal implementation details that explain nothing, repeated debugging logs, tool output, git operations unless the operation itself is the work.

**Accuracy rules — these outrank everything about wording:**

- Do not claim something was completed if the source only says it was researched, planned, or discussed.
- Keep research, planning, implementation, and testing distinct. A day spent investigating an approach is investigation, not delivery.
- If work was started but not finished, say so plainly ("Started on…", "Looked into…").
- If the log is ambiguous about whether something landed, prefer the weaker claim.
- Never invent work that is not in the source.

**Group by project** before writing either output. One log often spans several projects.

## Stage 2 — Notion version (detailed)

The permanent record. Write this first, and write it to be read later by someone who has forgotten the day — including you.

**Depth: 5–10 points per project.** More detail than Slack, always.

Each point should carry enough context to stand on its own:

- What was done, and what problem it solved
- Implementation detail where it materially explains the work (the approach taken, the cause of a bug, why a decision went one way)
- Testing performed and what it covered
- Investigation findings, including dead ends — a ruled-out approach is valuable later
- The outcome or current state

Unlike Slack, related work is **not** aggressively merged here. If three separate things were fixed, that is three points. Granularity is the point of this record.

### Notion format

```
Project Name — Xh

• Detailed point with context and outcome
• Detailed point with context and outcome
• Detailed point with context and outcome
```

One block per project. Include hours per project.

Still exclude commit hashes, branch names, and PR numbers — "detailed" means richer explanation of the work, not raw git metadata.

## Stage 3 — Slack version (condensed)

Derived from the Notion version, at a higher altitude.

**Never simply copy the Notion version into Slack.** Copying is the failure mode this stage exists to prevent. Slack points are rewritten shorter and flatter: merge related Notion points into single outcomes, drop the supporting detail, and keep only what a teammate skimming stand-up actually needs.

**Depth: 3–6 points per project.** Always fewer than Notion. If Slack has as many points as Notion, the condensing did not happen — go back and merge.

Selection rule: keep the most meaningful accomplishments. Minor fixes, small config changes, and incidental work belong in Notion and are dropped or folded into a broader point here.

### Slack format

```
**NOTION: Xh | Other: Xh**
**Project Name:(Xh) (Xh in Notion)**

* Humanised task
* Humanised task
* Humanised task
```

One `NOTION: Xh | Other: Xh` header at the top, then one heading block per project.

**Notion links.** Where a Notion page or entry link is available, include it so readers can reach the detail. If no link is available, leave it out rather than inventing one.

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

The question Slack answers is:

> What did I actually get done today?

Not:

> What files, commits, branches, or technical operations did I perform?

## Hours

Hours are usually not present in an agent log. Use hours stated in the source or given by the user. If they are missing, leave the `Xh` placeholders in place and tell the user which numbers to fill in — never estimate or invent hours.

## Example

Raw agent output containing:

> "Commit 3da5258 updated search logic, changed templates/search.json, fixed Shopify fuzzy matching, then commit 8035ba5 fixed three-column layout…"

**Notion version:**

```
Search Experience — Xh

• Reworked the search logic so exact matches rank above fuzzy results, fixing cases where a precise product name returned unrelated items.
• Fixed fuzzy matching returning no results for partial and misspelled queries.
• Added proper empty-state handling so a search with no matches shows a clear message instead of a blank page.
• Fixed the three-column grid breaking alignment when product titles ran to two lines.
• Corrected product card display so image, title, and price stay consistent across result rows.
• Tested the full search flow on desktop and mobile, covering exact, fuzzy, and no-result queries.
```

**Slack version:**

```
* Improved the search page to properly handle exact matches, fuzzy results, and empty states.
* Fixed the search grid layout and product card display.
* Tested the updated search experience across desktop and mobile.
```

Note the relationship: six Notion points became three Slack points. The first three Notion points merged into one Slack outcome, the next two merged into another, and testing stayed separate because it was genuinely done. Nothing was copied across verbatim.

## Before returning

- Every point in both outputs traces back to something real in the source.
- Notion is meaningfully more detailed than Slack — more points, more context.
- Slack is not a copy of Notion; points were merged and shortened.
- Research and planning are not described as shipped work.
- No commit hashes, branch names, PR numbers, or file paths remain in either output.
- No square brackets around Slack bullets.
- Both read like a person wrote them.
