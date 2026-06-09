---
name: coding-agent-handoff
description: >-
  Package the current chat conversation — a plan, implementation spec, research
  conclusion, bug investigation, or any scoped task — into a self-contained markdown
  brief that a coding agent (Claude Code, Codex, Cursor, or similar) can pick up and
  execute in the user's repository. The brief is produced as a downloadable artifact /
  file written for the coding agent as its reader: it carries the decisions, research,
  and rationale from this chat (which the agent cannot see), and tells the agent how to
  orient itself in the real codebase instead of trusting file paths and assumptions that
  were never verified against actual files. Works the same on claude.ai and ChatGPT,
  which both support the Agent Skills standard. Use this whenever the user asks to hand
  this off to a coding agent / Claude Code / Codex / Cursor, package or write this up for
  their coding agent, turn the discussion into a task/spec/brief for an agent to build,
  "ship this to my coding agent", or otherwise move work out of this conversation and
  into a coding tool to implement.
---

# Coding Agent Handoff

## What this skill is for

A chat assistant (claude.ai, ChatGPT) is where the user thinks; a coding agent
(Claude Code, Codex, Cursor, and the like) is where they build. Each side holds
something the other lacks, and a good handoff moves exactly the right thing across
the gap:

- **This conversation** holds context the coding agent can't reconstruct — the
  decision that was reached, the research that justified it, the alternatives that
  were weighed and rejected, the API shape sketched out, the edge cases discussed,
  the reasoning behind it all.
- **The coding agent** holds something this chat never had: the actual repository —
  real files, real structure, the ability to run `git status`, grep, and tests.

The brief you produce transmits the first across the gap and tells the agent to go
discover the second itself. Get this split right and the rest follows.

This skill is deliberately agent-agnostic. Don't assume a specific tool, OS, or
command set — write the brief so it's just as usable whether the user pastes it into
Claude Code, Codex, Cursor, or anything else. Lean on what's universal (read the
files, run the tests, follow existing conventions) rather than tool-specific flags.

## The principle that shapes everything: this chat is blind to the repo

You cannot see the user's filesystem. Every claim about their codebase that came up
in this conversation — a file path, a function name, "the code currently does X" —
is a **hypothesis**, not a fact. It may be from memory, from an outdated paste, or
from a reasonable guess. If the brief states these as fact, the coding agent will
trust them, edit the wrong thing, or build on a mental model that doesn't match
reality.

So the single most important habit in this skill is to **separate what's
authoritative from what needs verifying**:

- **Authoritative** — what the user decided, what they want, the constraints they
  chose, the research conclusions reached here. The agent should treat these as
  given.
- **To be verified** — anything about where code lives or how it currently behaves.
  Frame these as leads, and tell the agent to confirm them against real files before
  acting.

This is why the brief always opens by telling the agent to get its bearings first.

## Before you write: a quick gather

You don't need to interrogate the user — this skill triggers when they've already
done the thinking and want it packaged. But take a beat to make sure the brief won't
be hollow. Mentally check you can answer:

- What does success look like, concretely?
- What did the user actually decide (vs. options still open)?
- Is there anything they clearly must not break or change?

If one or two genuinely critical facts are missing — for instance you have no idea
what stack the repo is or how tests are run — either ask the user a quick question
or, better, hand it to the agent to discover ("find how this project runs its
tests"). Discovery is exactly what a coding agent is good at, so prefer marking
something for discovery over blocking on a question. Don't stall a good handoff over
details the agent can find in thirty seconds.

## Produce the brief as an artifact / file

Create the brief as a **markdown artifact or downloadable file** so the user can read
it inline, save it into their repo (as e.g. `TASK.md`), or copy it. Title it after
the task. Use the structure below.

After creating it, tell the user in one line how to use it — both paths work
regardless of which agent they use:
> Save it into your repo and tell your coding agent to read and implement it, or
> paste it as your first message in a fresh agent session.

## Brief structure

This is a starting template, not a form to fill rigidly. Keep the sections that
carry weight for this task; drop ones that would just hold filler. A research→action
handoff leans on *Context*; a bug fix leans on *Where this lives* and *Done when*.
Adapt.

```markdown
# <Task title>

> Handoff from a chat conversation. Get your bearings in the actual codebase before
> changing anything — the "where it lives" notes below are my best guess from our
> discussion, not verified against your files. Confirm them first.

## Goal
<1–3 sentences a fresh reader understands. What does success look like?>

## Context from our conversation
<The decisions, research conclusions, and reasoning that led here — the things you
can't get from the repo. Include alternatives we rejected and why, so you don't
re-propose them. Include concrete artifacts we produced: API shapes, example
payloads, schemas, formulas, specific edge cases.>

## Decisions already made — don't relitigate
- <firm choice + one-line why>

## Where this probably lives (verify before editing)
<Descriptive leads, not asserted paths: "likely the auth middleware", "the component
that renders the dashboard". Tell yourself to grep/read to confirm before touching.>

## What to do
1. Orient: <read the relevant area, run git status, confirm the leads above>
2. <concrete step>
3. <concrete step>

## Constraints
- <keep changes scoped to X / don't break Y / follow existing conventions>

## Done when
- <observable criteria: a test passes, the behavior works, the build is green>

## Open questions
- <anything unresolved — note whether to ask the user or use your judgment>
```

## Quality bar

A strong brief is one the coding agent can act on without ever seeing this chat:

- **It carries the why, not just the what.** "Build a rate limiter" is weak. "Build a
  rate limiter; we chose a token bucket over a fixed window because the user needs to
  allow short bursts — see the parameters we settled on below" is a handoff.
- **It never fakes repo knowledge.** Leads are framed as leads. The orient-first
  instruction is always there. This is the difference between a brief that helps and
  one that sends the agent confidently in the wrong direction.
- **It's agent-neutral.** No tool-specific assumptions. Anything about discovering or
  running things is phrased so any coding agent in any repo can follow it.
- **It's lean.** Distill the conversation, don't dump it. If a detail won't change
  what the agent does, it probably doesn't belong.
- **It closes the loop.** *Done when* tells the agent when to stop; *Open questions*
  tells it where to ask rather than guess.

## A quick contrast

**Weak** (asserts unverified state, no reasoning, no exit):
> Update the `validateUser` function in `src/auth/user.js` to also check the account
> status. Add the check and you're done.

**Strong** (authoritative intent, leads marked, reasoning carried, verifiable):
> ## Goal
> Block logins from suspended accounts. Right now suspended users can still log in;
> they shouldn't.
>
> ## Context from our conversation
> We decided to gate this at user validation rather than in the login route, because
> we found the same validation path is reused by the token-refresh flow — fixing it in
> one place covers both. "Suspended" means `status === 'suspended'`; banned and deleted
> are already handled elsewhere, so don't touch those.
>
> ## Where this probably lives (verify before editing)
> Likely a user-validation function (we guessed `validateUser`, maybe under an auth
> module) — grep for where login and token refresh both validate the user, and confirm
> it's the shared path before editing.
>
> ## Done when
> A suspended user is rejected on both fresh login and token refresh; existing auth
> tests still pass, and there's a test covering the suspended case.
