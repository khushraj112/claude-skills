# claude-skills

My personal Claude Code skills repository. Skills here are reusable across projects and installed globally rather than per-repo.

## Skills

- **`daily-slack-update`** — turns raw Claude Code / AI-agent work logs into two outputs: a detailed **Notion** work record (the source of record, with full context, testing and outcomes) and a condensed **Slack** stand-up update derived from it. Extracts what was actually accomplished and drops commits, branches, file paths and tool noise.

More skills will be added over time.

## Install

```bash
npx skills add khushraj112/claude-skills -g --skill daily-slack-update
```
