# AGENTS.md

A single [Agent Skill](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) that turns a chat conversation into a brief a coding agent can build from. Installs on claude.ai and ChatGPT; also works in Claude Code / Codex.

## Files

- `SKILL.md` — the skill. Keep it minimal; small skills generalize better across tasks.
- `README.md` — what it is, install, use. Keep it short and to the point.
- `banner.svg` — animated banner (light theme, ~12s loop). Validate XML after editing.
- `coding-agent-handoff.zip` — built artifact, **tracked in git** so users can download and upload it directly.

## Working in this repo

- After changing `SKILL.md`, `README.md`, or `banner.svg`, **rebuild the zip** in the same commit so the download never goes stale:
  ```bash
  zip -r coding-agent-handoff.zip coding-agent-handoff \
    -x "coding-agent-handoff/.git/*" "coding-agent-handoff/coding-agent-handoff.zip" \
       "coding-agent-handoff/.gitignore" "coding-agent-handoff/AGENTS.md" "coding-agent-handoff/CLAUDE.md"
  ```
  (run from the parent directory; the folder must sit at the archive root)
- The zip must contain only `SKILL.md`, `README.md`, `banner.svg` — exclude `.git`, `.gitignore`, the zip itself, and these repo-meta files (`AGENTS.md`, `CLAUDE.md`).
