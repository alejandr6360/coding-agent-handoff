---
name: claude-code-handoff
description: >-
  Package the current claude.ai conversation — a plan, implementation spec, research
  conclusion, bug investigation, or any scoped task — into a self-contained markdown
  brief that Claude Code can pick up and execute in the user's repository. The brief
  is produced as a downloadable artifact written for Claude Code as its reader: it
  carries the decisions, research, and rationale from this chat (which Claude Code
  cannot see), and tells Claude Code how to orient itself in the real codebase instead
  of trusting file paths and assumptions that were never verified against actual files.
  Use this whenever the user asks to hand this off to Claude Code, package or write this
  up for Claude Code, turn the discussion into a task/spec/brief for Claude Code, "ship
  this to Claude Code", or otherwise move work out of this conversation and into Claude
  Code to implement.
---

# Claude Code Handoff

## What this skill is for

claude.ai is where the user thinks; Claude Code is where they build. Each side
holds something the other lacks, and a good handoff moves exactly the right thing
across the gap:

- **This conversation** holds context Claude Code can't reconstruct — the decision
  that was reached, the research that justified it, the alternatives that were
  weighed and rejected, the API shape sketched out, the edge cases discussed, the
  reasoning behind it all.
- **Claude Code** holds something this chat never had: the actual repository — real
  files, real structure, the ability to run `git status`, grep, and tests.

The brief you produce transmits the first across the gap and tells Claude Code to
go discover the second itself. Get this split right and the rest follows.

## The principle that shapes everything: this chat is blind to the repo

You cannot see the user's filesystem. Every claim about their codebase that came up
in this conversation — a file path, a function name, "the code currently does X" —
is a **hypothesis**, not a fact. It may be from memory, from an outdated paste, or
from a reasonable guess. If the brief states these as fact, Claude Code will trust
them, edit the wrong thing, or build on a mental model that doesn't match reality.

So the single most important habit in this skill is to **separate what's
authoritative from what needs verifying**:

- **Authoritative** — what the user decided, what they want, the constraints they
  chose, the research conclusions reached here. Claude Code should treat these as
  given.
- **To be verified** — anything about where code lives or how it currently behaves.
  Frame these as leads, and tell Claude Code to confirm them against real files
  before acting.

This is why the brief always opens by telling Claude Code to get its bearings first.

## Before you write: a quick gather

You don't need to interrogate the user — this skill triggers when they've already
done the thinking and want it packaged. But take a beat to make sure the brief won't
be hollow. Mentally check you can answer:

- What does success look like, concretely?
- What did the user actually decide (vs. options still open)?
- Is there anything they clearly must not break or change?

If one or two genuinely critical facts are missing — for instance you have no idea
what stack the repo is or how tests are run — either ask the user a quick question
or, better, hand it to Claude Code to discover ("find how this project runs its
tests"). Discovery is exactly what Claude Code is good at, so prefer marking
something for discovery over blocking on a question. Don't stall a good handoff over
details Claude Code can find in thirty seconds.

## Produce the brief as an artifact

Create the brief as a **markdown artifact** so the user can read it inline, download
it as a file (to drop into their repo as e.g. `TASK.md`), or copy it. Title the
artifact after the task. Use the structure below.

After creating it, tell the user in one line how to use it — both paths work:
> Save it into your repo and tell Claude Code to read and implement it, or paste it
> as your first message in a Claude Code session.

## Brief structure

This is a starting template, not a form to fill rigidly. Keep the sections that
carry weight for this task; drop ones that would just hold filler. A research→action
handoff leans on *Context*; a bug fix leans on *Where this lives* and *Done when*.
Adapt.

```markdown
# <Task title>

> Handoff from a claude.ai conversation. Get your bearings in the actual codebase
> before changing anything — the "where it lives" notes below are my best guess from
> our discussion, not verified against your files. Confirm them first.

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

A strong brief is one Claude Code can act on without ever seeing this chat:

- **It carries the why, not just the what.** "Build a rate limiter" is weak. "Build a
  rate limiter; we chose a token bucket over a fixed window because the user needs to
  allow short bursts — see the parameters we settled on below" is a handoff.
- **It never fakes repo knowledge.** Leads are framed as leads. The orient-first
  instruction is always there. This is the difference between a brief that helps and
  one that sends Claude Code confidently in the wrong direction.
- **It's lean.** Distill the conversation, don't dump it. If a detail won't change
  what Claude Code does, it probably doesn't belong.
- **It closes the loop.** *Done when* tells Claude Code when to stop; *Open questions*
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
