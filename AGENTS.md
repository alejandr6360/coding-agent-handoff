# AGENTS.md

A single [Agent Skill](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) that turns a chat conversation into a brief a coding agent can build from. Installs on claude.ai and ChatGPT; also works in Claude Code / Codex.

## Files

- `SKILL.md` — the skill. Keep it minimal; small skills generalize better across tasks.
- `README.md` — what it is, install, use. Keep it short and to the point.
- `banner.svg` — animated banner (light theme, ~12s loop). Validate XML after editing.
- `coding-agent-handoff.zip` — built artifact, **tracked in git** and attached to each release so users can download and upload it directly.
- Repo meta (not part of the skill): `AGENTS.md`, `CLAUDE.md`, `LICENSE`, `CHANGELOG.md`.

## Working in this repo

- After changing `SKILL.md`, `README.md`, or `banner.svg`, **rebuild the zip** in the same commit so the download never goes stale. Build it from an explicit allowlist so repo-meta files can never leak in — the zip must contain only those three files, with the folder at the archive root:
  ```bash
  rm -f coding-agent-handoff/coding-agent-handoff.zip
  zip coding-agent-handoff/coding-agent-handoff.zip \
    coding-agent-handoff/SKILL.md coding-agent-handoff/README.md coding-agent-handoff/banner.svg
  ```
  (run from the parent directory)
- On a new release, bump `CHANGELOG.md`, tag `vX.Y.Z`, and attach the rebuilt zip to the GitHub release.
