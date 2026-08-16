# Working in this repository

This repo is the **developer-agent kit**: the Claude configuration that gets copied into
other projects. It is configuration only — it ships no `src/`, no `tests/`, no `data/`,
and no tool is ever built here or in a subdirectory of it.

These instructions apply to this repository only. They are not part of what the template
ships — `initialize-tool` deletes this file from every copy it creates.

## This clone is the maintainer's copy — no tool is built in it

Someone who PULLED this repo to use it is in a different situation: their copy becomes
their tool (init deletes this file as its first act), or they have it added to a project
they already have via the `adopting-a-project` skill. Both are correct for them.

In THIS clone, neither applies: it is where the kit is maintained. If asked to build a
tool here, say so and offer the two real options — set the configuration up in a new
folder OUTSIDE this repo (never a subdirectory), or `adopting-a-project` against the
repo they actually mean. "Just
quickly try it here" gets the same answer: a tool started in this folder corrupts the
thing every future tool is copied from. Throwaway sandboxes go under a scratch
directory, never in the working tree.

## The shipped files are artifacts, not instructions

`CLAUDE.md`, `README.md`, `CHANGELOG.md`, `LICENSE`, `docs/`, and `.claude/skills/`
are the **product**. In particular, this repo's
`CLAUDE.md` addresses the agent inside a *deployed tool* — it is not your brief here,
and its `<… set at init>` placeholders are deliberate and must stay. Editing any of
these files changes what every future tool receives, so treat each edit as a release,
not a note to self.

## What work does belong here

Improving the kit: its eleven skills, the hard rules, the init flow, the permission
allowlist, the docs conventions — plus testing those changes in a throwaway sandbox
(fake `HOME`, a scratch copy of this repo, never the real `~/.claude`).

House rules for that work:

- `CLAUDE.md` stays at 85 lines or fewer. It is loaded in full every session, and the
  rule set has already grown from 7 to 13 — when a rule needs more than a trigger, the
  detail belongs in the skill it names, not here.
- Any change to a shipped skill bumps its integer `version:` — that number is the only
  thing `syncing-the-kit` compares when a deployed tool asks for updates.
- Nothing this kit ships may install, write or leak outside the project it lives in.
  `~/.claude` is DENIED in `.claude/settings.json`; there is no machine-wide block and
  no shared skills directory. A tool's behaviour must never follow the user into their
  other repositories.
- `docs/journal/` ships empty by design, so this repo's own history lives in commit
  messages — write them for the next maintainer.
- Verify claims before making them: run the greps, run the sandbox. The template
  demands honest reporting of the tools built from it; it holds here first.
