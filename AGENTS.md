# Working in this repository

This repo is the **developer-agent template**: the Claude configuration that gets copied
into other projects. It is not a tool, and no tool is ever built here.

These instructions apply to this repository only. They are not part of what the template
ships — `initialize-tool` deletes this file from every copy it creates.

## Never build a user's tool in this repo

If the user asks to start a new tool or project — anything that would put their code,
their data, or their features into this folder — do not do it here. Instead:

1. Ask which directory the new tool should live in.
2. Create that directory and copy everything from this repo into it EXCEPT `.git/` and
   `AGENTS.md`. (Writing outside this repo shows a permission box; say in one sentence
   what you are copying and where.)
3. If that directory already holds a project, stop and say so — dropping this
   configuration into an existing project is not something the setup handles yet.
4. Tell the user to open the new folder in VSCode and say hello there; init runs in that
   copy, never here.

Refuse the same way for "just quickly try it here" — a tool started in this folder
corrupts the thing every future tool is copied from.

## The shipped files are artifacts, not instructions

`CLAUDE.md`, `README.md`, `CHANGELOG.md`, `LICENSE`, `docs/`, and
`.claude/skills/_user-level/` are the **product**. In particular, this repo's
`CLAUDE.md` addresses the agent inside a *deployed tool* — it is not your brief here,
and its `<… set at init>` placeholders are deliberate and must stay. Editing any of
these files changes what every future tool receives, so treat each edit as a release,
not a note to self.

## What work does belong here

Improving the template: the seven skills, the hard rules, the init flow, the permission
allowlist, the docs conventions — plus testing those changes in a throwaway sandbox
(fake `HOME`, a scratch copy of this repo, never the real `~/.claude`).

House rules for that work:

- Any change to a shipped skill bumps its integer `version:`; deployed machines only
  receive changes when the number goes up. A change to the machine-wide block bumps the
  block version in `initialize-tool`.
- `docs/journal/` ships empty by design, so this repo's own history lives in commit
  messages — write them for the next maintainer.
- Verify claims before making them: run the greps, run the sandbox. The template
  demands honest reporting of the tools built from it; it holds here first.
