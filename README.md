<div align="center">

![claude-code-handoff banner](./banner.svg)

# claude-code-handoff

**A Claude Skill that turns a claude.ai conversation into a brief Claude Code can actually build from.**

</div>

---

You do your thinking on **claude.ai** — planning a feature, weighing approaches, chasing down a bug. Then comes the awkward part: getting all of that into **Claude Code**, where the real repo lives and the work actually happens. Copy-pasting the chat loses the decisions. Starting fresh loses the research. And anything you *tell* Claude Code about your files is a guess, because claude.ai never saw them.

This skill closes that gap. On claude.ai, you say *"hand this off to Claude Code"* and it produces a clean **markdown brief** (as a downloadable artifact) that:

- **carries the irreplaceable part** — the decisions you made, the research you did, the alternatives you rejected and why, the API shapes and edge cases you nailed down;
- **never fakes repo knowledge** — every claim about where code lives or how it behaves is framed as a *lead to verify*, not a fact, because the conversation was blind to your filesystem;
- **tells Claude Code to orient first** — read the real files, confirm the leads, *then* execute — so it doesn't charge confidently in the wrong direction.

The result is a first message you hand to Claude Code that's self-contained, honest about what's certain vs. assumed, and ends knowing what "done" looks like.

## What the brief looks like

The skill adapts the structure to the task, but the shape is:

```markdown
# <Task title>

> Handoff from a claude.ai conversation. Get your bearings in the actual
> codebase before changing anything — the "where it lives" notes are my best
> guess from our discussion, not verified against your files.

## Goal                              — what success looks like
## Context from our conversation     — the decisions + research you can't get from the repo
## Decisions already made            — firm choices, don't relitigate
## Where this probably lives         — leads to verify, never asserted paths
## What to do                        — orient first, then concrete steps
## Constraints                       — what to keep scoped / not break
## Done when                         — observable, verifiable criteria
## Open questions                    — ask vs. use judgment
```

## Install on claude.ai

Skills are uploaded as a `.zip` of the skill folder. Quick version:

1. **Grab the zip** — use the bundled `claude-code-handoff.zip`, or zip the folder yourself so the folder sits at the *root* of the archive (not just its contents):
   ```bash
   zip -r claude-code-handoff.zip claude-code-handoff
   ```
2. In **claude.ai**, open **Settings → Capabilities** (the **Skills** section). *(Custom skill upload requires a Pro/Max/Team/Enterprise plan with skills enabled.)*
3. Click **+ → Create skill**, then **upload** `claude-code-handoff.zip`.
4. Toggle the skill **on**. Done.

> The `name` in the skill's frontmatter must be lowercase letters, numbers, and hyphens — this one (`claude-code-handoff`) already is, so it'll upload cleanly.

For the official walkthrough, see Anthropic's guides: [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) and [How to create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills).

## How to use it

When you're ready to move work from a chat into your editor, say something like:

- *"Package this up for Claude Code to build."*
- *"Write this up so Claude Code can do the migration."*
- *"Hand this off to Claude Code to fix."*

The skill triggers on explicit handoff requests like these. It produces the brief as an artifact, then you have two ways to use it:

- **Save it into your repo** (e.g. as `TASK.md`) and tell Claude Code: *"read TASK.md and implement it."*
- **Paste it** as your first message in a fresh Claude Code session.

Either way, Claude Code starts with your full context — and the good sense to check it against the real code before it touches anything.

## What's in here

```
claude-code-handoff/
├── SKILL.md      # the skill itself
├── README.md     # this file
└── banner.svg    # animated banner
```

Works anywhere skills run — claude.ai, Claude Code, and the API — but it's designed for the claude.ai → Claude Code direction.
