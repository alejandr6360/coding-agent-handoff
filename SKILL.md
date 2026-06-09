---
name: coding-agent-handoff
description: >-
  Turn the current chat conversation into a self-contained markdown brief (a saved file
  or artifact) that a coding agent — Claude Code, Codex, Cursor, or similar — can execute
  in the user's repository. Use when the user asks to hand
  this off to a coding agent / Claude Code / Codex, write it up as a task or spec to build,
  or otherwise move the work from this chat into a coding tool to implement.
---

# Coding Agent Handoff

Write the conversation up as a markdown brief the user can save into their repo (e.g. `TASK.md`).
Present it as an artifact/file and tell them both ways to use it.
At the top append brief directions for a coding agent.
If unsure on what the coding agent instructions are supposed to be, clarify with the user. Do not guess except in the most obvious cases.

The current session most likely can't see the user's machine, so carry what's certain — the decisions, research,
rejected options, and reasoning from this conversation.
Given that, anything you say about existing code might be a guess: state it as an assumption in brief for the agent to verify
before relying on it, disregard if the user is starting a new project and continue with the assumptions made so far.
Let the agent set things up. Keep it lean. Including ideas and directions is key. Think of this like a context transfer.
Don't force the agent into strict workflows unless required like: read the code, run the tests, follow conventions, etc.

Adapt this template for your conversation so far:

```markdown
# <task title>
> Handoff from a chat. Verify any assumptions about existing code before changing it.

## Goal — what success looks like
## Context — decisions, research, rejected options, and the "why".
## Steps — what to do, do not present strict workflows unless required.
## Done when — observable criteria
## Open questions — ask vs. decide
```
