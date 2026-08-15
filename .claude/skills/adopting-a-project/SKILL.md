---
name: adopting-a-project
description: Use when the user wants this developer-agent setup added to a project they already have, or when they start working on code that lives in another folder — installs the configuration into their repo without breaking what is already there.
version: 3
---

# Adopting an Existing Project

Their project comes first. You are adding a way of working to a codebase that already
works; nothing of theirs may be lost, overwritten, or silently changed.

## Work in THEIR folder, not from the kit

Everything below happens in the user's project, with their git. If this session is
still sitting in the starter-kit folder, copy `.claude/skills/` and
`.claude/settings.json` across as the first act, then ask the user to open THEIR project
in VSCode and say hello — the rest of this skill runs there, where it can see their
files and their history. Running it at a distance from the kit folder branches the wrong
repo and leaves their work with no undo.

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

- `.claude/skills/` — the skills themselves, copied into their project so this way of
  working exists only where they asked for it. Never installed machine-wide.
- `.claude/settings.json` — the permission rules. If they already have one, MERGE: keep
  every entry of theirs, add only what is missing, never remove or reorder theirs. One
  trap to check explicitly: an entry added to `ask` OUTRANKS an existing `allow`, so
  adding this setup's `ask` list can turn commands they run freely today (`git push`,
  `curl`, `rm`) into a prompt every time. List those before merging and let them choose
  per entry — silently making a developer's daily commands prompt is a downgrade, not a
  safeguard.
- `CLAUDE.md` — the hard rules and conventions. If they already have one, do NOT
  replace it: append the rules inside a clearly delimited block
  (`<!-- developer-agent:start vN -->` … `<!-- developer-agent:end -->`), leaving their
  content untouched. Drop the parts that do not apply to their project (the stack
  policy, if they already have a stack).
- `docs/INDEX.md`, `docs/TAGS.md`, `docs/journal/INDEX.md` — create only the ones that
  are missing, and never carry the kit's `<!-- UNINITIALIZED -->` marker into them: this
  project is not a fresh tool and must not trigger first-run setup. If they already keep docs somewhere else, adapt the paths to their layout
  instead of imposing this one, and say so.
- `.gitignore` — append only the lines they lack.

Never added to someone else's project: `README.md`, `LICENSE`, `AGENTS.md`, `src/`,
`tests/`, or anything from the starter kit's own history. `CHANGELOG.md` only if they
want one — if they decline, say so in the delimited block you add to their `CLAUDE.md`
("this project keeps no changelog"), so the documenting rule does not later demand a
file that was deliberately refused.

Also drop a `.developer-agent.json` at their root recording `role: "adopted"`,
`kit_origin` and `kit_version`, so a later session knows what was added and
`syncing-the-kit` can offer updates.

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
