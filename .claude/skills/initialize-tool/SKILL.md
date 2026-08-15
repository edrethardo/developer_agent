---
name: initialize-tool
description: Use when docs/journal/INDEX.md contains the UNINITIALIZED marker — first-run setup; interview the user, set up git, install shared skills, configure the machine.
version: 7
---

# Initialize Tool

First-run setup. Entirely conversational — the user never opens a terminal. Reply in the
language of the user's first message throughout.

Every step is idempotent: each has a done-check and is skipped when already satisfied, so
an interrupted init simply resumes next conversation. The `<!-- UNINITIALIZED -->` marker
in `docs/journal/INDEX.md` is removed LAST, and ONLY when every step below actually
succeeded — marker present means init is incomplete. If ANY step is blocked (a denied
permission, a missing tool, an unreachable path), you have not finished: leave the
marker in place, tell the user plainly which parts are pending and what you need from
them, and stop. Never announce that setup is complete while the marker's preconditions
are unmet — the marker is the only thing that brings init back, so removing it early
strands the tool half-configured forever.

Never widen your own permissions to get unblocked: do not edit any `settings.json`
(this project's or `~/.claude/`) to grant yourself a path or command. A permission box
is the user's decision point — ask them to approve it in plain words, or stop and
report. A session that grants itself rights has removed the user's only safeguard.

(Edge case: init edits sitting uncommitted with NO marker means init crashed between its
last two actions — just commit them as `Initialize <tool name>` and move on.)

## Step 0 — which of the three situations is this?

An `AGENTS.md` at the root means this folder is a fresh copy of the starter kit and
nothing has been decided yet. Ask the user once, in plain language, which they want:

a. **Build a new tool right here** — this folder becomes their tool. Delete `AGENTS.md`
   (it belongs to the starter kit, not to a tool) and continue with step 1.
b. **Add this way of working to a project they already have** — do NOT initialize here,
   and do not work on their project from this folder either. Ask for its path, copy
   `.claude/skills/` and this repo's `.claude/settings.json` into it as described by
   `adopting-a-project` (read that file directly — it is a skill of THIS folder and will
   not be loadable once they move), then tell them to open THEIR project in VSCode and
   continue there. Everything else in that skill happens in their folder, with their
   git.
c. **They are improving the starter kit itself** (maintainer) — do not initialize at
   all; follow `AGENTS.md` and stop here.

If they later start doing work that clearly belongs to another project — editing code
somewhere else, asking for features in a codebase they already have — offer (b) again
rather than working at a distance: `adopting-a-project`.

## Steps, in order

### 1. Prerequisites

Detect the OS from the environment. Check git: run `git --version`.
If git is missing: explain in plain words what git is (the change-history system this
setup depends on) and guide the user through installing it for THEIR OS — this genuinely
needs their clicks on an installer; say so honestly. Do not continue without git.
Windows note: git installed while VSCode is running is often not visible until VSCode
fully restarts. If `git --version` still fails after an install, say so, ask the user to
close and reopen VSCode, and let the marker bring init back next conversation.
If Bash commands THEMSELVES fail (a native Windows machine without git may give Claude
Code no working shell at all), do not loop on shell errors: judge the OS from the
environment info you already have and go straight to the plain-words walkthrough for
installing "Git for Windows", then the full VSCode restart.

### 2. Interview — one question per message

Ask and remember the answers — the file writes all happen in step 3. Skip any question
whose answer is already recorded in CLAUDE.md.

a. Preferred language.
b. What should this tool do? A short paragraph is enough — details come per-feature later.
c. What should the tool be called?

### 3. Mechanical setup (narrate briefly as you go)

Done-checks in parentheses; skip satisfied steps:

- Git repo exists (`.git/` present): else `git init -b main`.
- Git identity works (BOTH `git config user.name` AND `git config user.email` print
  something — an identity someone already configured machine-wide is fine): else set the
  missing ones REPO-LOCALLY — `git config user.name "<tool name>"`, `git config
  user.email "<tool-slug>@local.invalid"`. Never `--global`; the user is not asked.
- Pristine baseline committed (`git log` shows at least one commit): else commit
  everything as "Initial commit: pristine template" BEFORE any personalization edit.
  If the tree was ALREADY personalized when this first commit happens (crash path),
  commit it as `Initial commit (post-crash, not pristine)` instead. Before EITHER
  commit, look at the tree: if it holds files the template does not ship (source in
  `src/` beyond `.gitkeep`, unknown top-level folders, someone's documents), STOP —
  this folder has prior work in it; show the user what you found and ask before
  committing anything. Never label such a tree pristine.
- CLAUDE.md personalized (no `<… set at init>` markers remain): write tool name, one-line
  description, user language.
- `docs/user/about.md` exists: write it in the user's language — what this tool is for,
  from the interview answer. Frontmatter per the `documenting` skill (tag: `docs`); add
  its line to `docs/INDEX.md` (replacing the `_(none yet)_` placeholder if present). If
  resuming and the interview answer is no longer available, re-ask question (b) rather
  than padding the one-liner.
- The setup placeholder under `## [Unreleased]` in CHANGELOG.md is gone (and the header
  is in the user's language): rewrite the header paragraph if needed, and replace the
  placeholder line with a first entry saying the tool was set up today.
- `README.md` describes THIS tool, not the starter kit: replace it with a short page in
  the user's language — what the tool does, how to ask for things, where the manual and
  the changelog are. The kit's own README is instructions for setting up, and they are
  finished.
- No `AGENTS.md` remains (it belongs to the template repo only, never to a tool): if
  one is present, delete it.
- Template build history pruned: delete exactly those `docs/journal/` entry files
  (never `INDEX.md`) that are byte-identical to a file in this repo's FIRST commit —
  those are the TEMPLATE's own memoirs, shipped with the copy; anything that differs is
  this tool's real work and stays. Remove the deleted entries' index lines too. The
  memoirs remain in the template repo and in the first commit. Deletion shows a
  permission box; say in one sentence why.
- Commit the personalization: `Personalize template for <tool name>` (skip if the tree
  is clean).

### 4. Permissions explainer

Skip if a `docs/journal/*-init.md` entry already exists (this ran before). One short
paragraph, user's language, own message, BEFORE the installs below cause any permission
boxes: routine work runs without asking; unusual actions show a permission box; a box
means "stop and consider" — the user can always ask "what does this mean?" and get a
plain answer first. Adding something to the always-allowed list is possible, and the
worst case gets explained before it happens (the allowlist rule in CLAUDE.md's hard
rules).

### 5. Make this repo self-contained

Nothing is installed outside this folder — no machine-wide files, no shared skills
directory. The skills in `.claude/skills/` came with the copy and are already this
tool's own; they load only here, so nothing this tool does can change how Claude behaves
in the user's other projects.

Two things to record so later sessions know what this repo is:

- Sever the tie to the kit if it is still there: if `git remote` shows an `origin`
  pointing at the developer-agent kit, remove it (`git remote remove origin`). This repo
  is the user's tool now, not a fork of the kit — and pulling the kit's history over
  their work would overwrite it. Say in one sentence that you did this and that updates
  come via the `syncing-the-kit` skill instead.
- Write `.developer-agent.json` at the root, committed with the personalization:

      {"role": "tool", "kit_origin": "https://github.com/edrethardo/developer_agent",
       "kit_version": "<the version: of initialize-tool>", "initialized": "YYYY-MM-DD"}

  `syncing-the-kit` reads this later; `kit_origin` becomes a local path instead if the
  user keeps a copy of the kit on disk.

### 6. Close out

- Write the first journal entry — file `docs/journal/YYYY-MM-DD-init.md` — per the
  `journaling` skill, documenting this init: answers given, versions installed, anything
  skipped. Skip if a `*-init.md` entry already exists.
- Check every preceding step really happened before going further: `docs/user/about.md`
  exists, no `<… set at init>` markers remain, the CHANGELOG placeholder is gone,
  `README.md` describes THIS tool rather than the starter kit, `AGENTS.md` is gone,
  template memoirs are pruned, `.developer-agent.json` exists, and no `origin` remote
  points at the kit. If any of these is not true, STOP here: keep the marker, list the
  pending items for the user in plain language, and say init will resume next
  conversation.
- Only if all of them hold: as a separate final edit (never combined with the index-line
  edit above), remove the `<!-- UNINITIALIZED -->` line from `docs/journal/INDEX.md` —
  the LAST file edit of init.
- Commit: `Initialize <tool name>` (skip if the tree is clean).
- Only now suggest renaming the folder to the tool's name: close VSCode, rename in the
  file manager, reopen. Skippable, and nothing breaks either way — nothing outside this
  folder records its path. An agent must not rename its own working directory.
- Hand over: "Setup's done. Tell me what you'd like the tool to do first."
