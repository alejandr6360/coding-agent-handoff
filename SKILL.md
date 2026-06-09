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
`TASK.md`) or paste as the coding agent's first message. Produce it as an artifact/file
and tell them both ways to use it.

The chat can't see the user's machine, so carry what's certain — the decisions, research,
rejected options, and reasoning from this conversation — as authoritative. Anything you
say about existing code is a guess: state it as an assumption for the agent to verify
before relying on it. If the user is starting a new project, there may be nothing to
locate yet — don't invent structure; let the agent set things up. Keep it lean and
tool-neutral: lean on universals (read the code, run the tests, follow conventions), not
specific commands.

Adapt this template — drop any section that doesn't apply:

```markdown
# <task title>
> Handoff from a chat. Verify any assumptions about existing code before changing it.

## Goal — what success looks like
## Context — decisions, research, rejected options, and the why (what's not in the repo)
## Steps — what to do
## Done when — observable criteria
## Open questions — ask vs. decide
```

For an existing codebase, you can add a `## Where it might live` section — but only as
leads for the agent to confirm, never as asserted paths. Skip it for a new project.
