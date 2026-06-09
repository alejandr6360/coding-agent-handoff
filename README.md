<div align="center">

![coding-agent-handoff banner](./banner.svg)

# coding-agent-handoff

**A skill that turns a chat conversation into a brief your coding agent can actually build from.**

Works on **claude.ai** and **ChatGPT** — both support the [Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) open standard, so one skill file installs in either. The brief it produces is agent-neutral: hand it to **Claude Code**, **Codex**, **Cursor**, or any coding agent.

</div>

---

You do your thinking in a **chat assistant** — planning a feature, weighing approaches, chasing down a bug. Then comes the awkward part: getting all of that into a **coding agent**, where the real repo lives and the work actually happens. Copy-pasting the chat loses the decisions. Starting fresh loses the research. And anything you *tell* the agent about your files is a guess, because the chat never saw them.

This skill closes that gap. You say *"hand this off to my coding agent"* and it produces a clean **markdown brief** (a downloadable artifact / file) that:

- **carries the irreplaceable part** — the decisions you made, the research you did, the alternatives you rejected and why, the API shapes and edge cases you nailed down;
- **never fakes repo knowledge** — every claim about where code lives or how it behaves is framed as a *lead to verify*, not a fact, because the conversation was blind to your filesystem;
- **tells the agent to orient first** — read the real files, confirm the leads, *then* execute — so it doesn't charge confidently in the wrong direction.

The result is a first message you hand to your coding agent that's self-contained, honest about what's certain vs. assumed, and ends knowing what "done" looks like.

## What the brief looks like

The skill adapts the structure to the task, but the shape is:

```markdown
# <Task title>

> Handoff from a chat conversation. Get your bearings in the actual codebase
> before changing anything — the "where it lives" notes are my best guess from
> our discussion, not verified against your files.

## Goal                              — what success looks like
## Context from our conversation     — the decisions + research you can't get from the repo
## Decisions already made            — firm choices, don't relitigate
## Where this probably lives         — leads to verify, never asserted paths
## What to do                        — orient first, then concrete steps
## Constraints                       — what to keep scoped / not break
## Done when                         — observable, verifiable criteria
## Open questions                    — ask vs. use judgment
```

## Install

Skills are uploaded as a `.zip` of the skill folder. The same zip works on both platforms.

**Build the zip** — use the bundled `coding-agent-handoff.zip`, or zip the folder yourself so the folder sits at the *root* of the archive (not just its contents):
```bash
zip -r coding-agent-handoff.zip coding-agent-handoff
```

**On claude.ai** — open **Settings → Capabilities** (the **Skills** section), click **+ → Create skill**, and upload the zip. Toggle it on.
> Docs: [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) · [How to create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)

**On ChatGPT** — open the **Skills** section, choose **New skill → Upload from your computer**, and select the zip. *(Skills are in beta on Business / Enterprise / Edu plans; a workspace admin may need to enable skill uploading.)*
> Docs: [Skills in ChatGPT](https://help.openai.com/en/articles/20001066-skills-in-chatgpt)

> The `name` in the skill's frontmatter must be lowercase letters, numbers, and hyphens — this one (`coding-agent-handoff`) already is, so it uploads cleanly to either platform.

**Using a coding agent directly?** Claude Code and Codex also read `SKILL.md` — drop the folder into the tool's skills directory and skip the upload step entirely.

## How to use it

When you're ready to move work from a chat into your editor, say something like:

- *"Package this up for my coding agent to build."*
- *"Write this up so Claude Code can do the migration."*
- *"Hand this off to Codex to fix."*

The skill triggers on explicit handoff requests like these. It produces the brief as an artifact, then you have two ways to use it:

- **Save it into your repo** (e.g. as `TASK.md`) and tell your agent: *"read TASK.md and implement it."*
- **Paste it** as your first message in a fresh coding-agent session.

Either way, the agent starts with your full context — and the good sense to check it against the real code before it touches anything.

## What's in here

```
coding-agent-handoff/
├── SKILL.md      # the skill itself
├── README.md     # this file
└── banner.svg    # animated banner
```
