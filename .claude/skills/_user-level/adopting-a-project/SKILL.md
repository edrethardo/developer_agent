---
name: adopting-a-project
description: Use when the user wants this developer-agent setup added to a project they already have, or when they start working on code that lives in another folder — installs the configuration into their repo without breaking what is already there.
version: 2
---

# Adopting an Existing Project

Their project comes first. You are adding a way of working to a codebase that already
works; nothing of theirs may be lost, overwritten, or silently changed.

## Before touching anything

1. Ask for the project's folder, and look at it: is it a git repo, does it have commits,
   is the working tree clean?
2. If it is not a git repo, say plainly that the setup depends on git for its undo
   story, and offer to run `git init` there — their choice.
3. If the tree is dirty, ask them to let you commit their pending work first (or stop
   until they have) — adopting on top of uncommitted changes makes the two impossible to
   separate later.
4. Do the whole adoption on a branch (`git checkout -b add-developer-agent`). Say why in
   one sentence: everything you add can be thrown away with one command until they are
   happy. Merge only when they say so.

## What gets added

Additive only:

- `.claude/settings.json` — the permission rules. If they already have one, MERGE:
  keep every entry of theirs, add only what is missing, never remove or reorder theirs.
- `CLAUDE.md` — the hard rules and conventions. If they already have one, do NOT
  replace it: append the rules inside a clearly delimited block
  (`<!-- developer-agent:start vN -->` … `<!-- developer-agent:end -->`), leaving their
  content untouched. Drop the parts that do not apply to their project (the stack
  policy, if they already have a stack).
- `docs/INDEX.md`, `docs/TAGS.md`, `docs/journal/INDEX.md` — create only the ones that
  are missing. If they already keep docs somewhere else, adapt the paths to their layout
  instead of imposing this one, and say so.
- `.gitignore` — append only the lines they lack.

Never added to someone else's project: `README.md`, `CHANGELOG.md`, `LICENSE`,
`AGENTS.md`, `src/`, `tests/`, or anything from the starter kit's own history.

The shared skills live in `~/.claude/skills/` and are installed once per machine, not
per project — install any that are missing, using the same version rule as first-time
setup (copy only when the shipped `version:` is greater; never downgrade; never touch
skills this setup did not ship). Then add the project to the machine-wide block's list.

## Collisions — ask, with an honest recommendation

For every file that already exists where the setup wants to write, stop and put the
choice to the user in plain language:

- what they have there now, in one sentence;
- what this setup would add, in one sentence;
- your recommendation and WHY — merge, skip, or (rarely) replace;
- the honest cost of each: what they lose, what breaks, what stays.

Recommend by default: **merge** for `settings.json` and `.gitignore`, **append a
delimited block** for an existing `CLAUDE.md`, **skip** anything of theirs you cannot
merge without judgment calls. Recommend replacing only when their file is an empty stub,
and say that is what you are seeing.

Where their existing setup CONTRADICTS these rules — a different commit convention, a
review workflow with pull requests, a docs system of their own — their project wins.
Say which rules you are dropping and why; do not smuggle a conflicting rule in and hope
it goes unnoticed.

## Finishing

- Show them what changed: files added, files merged, anything skipped.
- Tell them the branch name and that merging it is their call.
- Journal the adoption (`journaling` skill) — what was merged, what was skipped, what
  their project overrides — so a later session does not re-litigate it.
- Then offer an adversarial review of their project — once, as an offer, never as a
  condition of finishing. Put it plainly: you have just read your way into this
  codebase, and you can spend a while attacking its structure on purpose — boundaries,
  duplication, coupling, the places one change ripples through many files — and write
  down what you find. Say what it costs (a chunk of time and tokens, no code changes
  unless they ask) and what it is worth (the findings are ranked, and they decide what,
  if anything, gets fixed).
  Be honest about its limits when you make the offer: this is a first impression of a
  codebase you have known for minutes, so findings are claims to verify, not verdicts,
  and anything you flag may have a reason you have not seen yet. Ask the user what hurts
  today — the answer usually beats anything you would have guessed.
  If they accept: review, rank findings by likelihood times damage, journal them
  (`journaling` skill), and change nothing until they choose. If they decline, say the
  offer stands whenever they want it — and let the every-one-or-two-features cadence in
  the hard rules take over from there.
