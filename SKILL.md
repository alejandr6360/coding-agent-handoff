---
name: coding-agent-handoff
description: >-
  Turn the current chat conversation into a self-contained markdown brief (a saved file
  or artifact) that a coding agent — Claude Code, Codex, Cursor, or similar — can execute
  in the user's repository. Works on claude.ai and ChatGPT. Use when the user asks to hand
  this off to a coding agent / Claude Code / Codex, write it up as a task or spec to build,
  or otherwise move the work from this chat into a coding tool to implement.
---

# Coding Agent Handoff

Write the conversation up as a markdown brief the user can save into their repo (e.g.
`TASK.md`) or paste as the coding agent's first message. Produce it as an artifact/file,
then tell them those two ways to use it.

The chat can't see their files, so anything said here about where code lives or how it
works is a guess. Carry what's certain — the decisions, research, rejected options, and
the reasoning from this conversation — as authoritative. Frame anything about the codebase
as a lead the agent should verify against the real files first. Keep it lean and
tool-neutral: lean on universals (read the code, run the tests, follow existing
conventions), not specific commands or flags.

Adapt this template; drop any section that wouldn't earn its place:

```markdown
# <task title>
> Handoff from a chat. Confirm the "where it lives" notes against the real code before editing.

## Goal — what success looks like
## Context — decisions, research, rejected options, and the why (what's not in the repo)
## Where it probably lives — leads to verify, not asserted paths
## Steps — orient first, then what to do
## Done when — observable criteria
## Open questions — ask vs. decide
```
