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

Write the conversation up as a markdown brief the user can save into their repo (e.g. `TASK.md`)
or paste as the coding agent's first message. Present it as an artifact/file and tell them both ways to use it.
Start the brief with a short line of directions to the coding agent — the blockquote in the template below.
If unsure on what the coding agent instructions are supposed to be, clarify with the user. Do not guess except in the most obvious cases.

The current session most likely can't see the user's machine, so carry what's certain — the decisions, research,
rejected options, and reasoning from this conversation.
Given that, treat anything you say about existing code as an assumption: state it briefly and tell the agent to verify
it before relying on it. If the user is starting a new project there's nothing to verify — just carry the assumptions
made so far and let the agent set things up.
Keep it lean. Including ideas and directions is key. Think of this like a context transfer.
Don't force the agent into strict workflows unless required like: read the code, run the tests, follow conventions, etc.

Adapt this template for your conversation so far:

```markdown
# <task title>
> Handoff from a chat — build from the context below. Verify any assumptions about existing code before relying on them.

## Goal — what success looks like
## Context — decisions, research, rejected options, and the "why".
## Steps — what to do, do not present strict workflows unless required.
## Done when — observable criteria
## Open questions — ask vs. decide
```
