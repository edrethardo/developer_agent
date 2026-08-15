---
name: syncing-the-kit
description: Use when the user wants a newer version of the developer-agent setup, or when something in the way of working seems outdated — updates this repo's skills and rules from the kit without touching the tool's own code or history.
version: 1
---

# Syncing the Kit

This repo is the user's tool, not a fork of the starter kit. Never `git pull` the kit
into it: their history and the kit's are unrelated, and a merge would drop the kit's
`AGENTS.md`, `README.md` and old docs straight back on top of the tool's own files.

## How to update instead

1. Read `.developer-agent.json` for `kit_origin` and `kit_version`.
2. Get a copy of the kit somewhere OUTSIDE this repo — a scratch folder, a temp
   directory, the user's Downloads. If `kit_origin` is a local path, read from there. If
   it is a URL and there is no network access or the user does not want a download, say
   so plainly and stop; there is no shame in staying on the current version.
3. Compare skill by skill: for each folder in the kit's `.claude/skills/`, look at the
   integer `version:` in its `SKILL.md` against the copy in this repo's
   `.claude/skills/`. Newer in the kit → offer the update. Same or older → leave it.
4. Show the user what would change, in their language: which skills, what each change
   means for how you work, and anything that would conflict with how this tool already
   does things. Their answer decides; per-skill is fine.
5. Apply on a branch (`git checkout -b update-setup`), one commit, then merge when they
   are happy. Update `kit_version` in `.developer-agent.json`.

## What is never synced

The tool's own code, data, docs, journal, CHANGELOG or README; anything the user or a
past session deliberately changed in a skill (say so and keep their version unless they
choose otherwise); and the kit's `AGENTS.md`, `LICENSE` and starter-kit README, which
belong to the kit and never to a tool.

## Afterwards

Journal it (`journaling` skill): which skills moved to which version, what was declined
and why. That record is what stops the next session from re-offering something the user
already turned down.
