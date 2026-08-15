---
name: initialize-tool
description: Use when docs/journal/INDEX.md contains the UNINITIALIZED marker — first-run setup; interview the user, set up git, install shared skills, configure the machine.
version: 4
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

### 5. Install shared skills

First tell the user in one sentence: several permission boxes will now appear — each is
this setup copying one skill file into `~/.claude/skills/`, the shared folder every tool
on this machine reads (writes there are deliberately NOT pre-approved). Then, for each
folder in `.claude/skills/_user-level/`, target `~/.claude/skills/<name>/`:

- Not present there → copy it by READING each file and WRITING it to the target with the
  file tools; never shell out to `cp`.
- Present → compare the integer `version:` in both SKILL.md frontmatters; copy (the same
  way) only if the shipped version is GREATER. Never downgrade. Never touch skills this
  template didn't ship. When upgrading, also delete files in the target folder that the
  shipped folder no longer contains — again a permission box; tell the user in one
  plain sentence what is being removed and why before confirming.

### 6. Machine-wide CLAUDE.md block

File: `~/.claude/CLAUDE.md` — create it if missing. NEVER modify anything outside the
delimiters. Rules:

- No `<!-- developer-agent:start` block → append the block below to the end of the file,
  with the `- <tool name> — <absolute path>` placeholder line REPLACED by this tool's
  real entry.
- Block exists with a LOWER version in the start delimiter → replace the block's content
  (delimiter to delimiter) with the current template, preserving the existing tool list
  and any already-customized Template line. Concretely: write the new template, then
  replace its placeholder lines with the old block's values.
- Block exists with an EQUAL or HIGHER version → leave its content alone.
- Then ensure THIS tool is listed exactly once, keyed by absolute path: path already
  listed → leave it; otherwise append one line `- <tool name> — <absolute path>`.

Block template (indented here only to mark it as a template — write it UNINDENTED):

    <!-- developer-agent:start v3 -->
    ## Developer-agent tool repos

    Shared skills for these repos live in ~/.claude/skills/ (journaling,
    finding-knowledge, git-discipline, explaining-complexity, documenting,
    creating-skills, testing) — use them. They are a read-only deployment target: only a
    template's init/upgrade step writes there; tools improve skills via their
    creating-skills flow, never by editing ~/.claude/skills/ directly. Before
    investigating anything in one of these repos, read its docs/journal/INDEX.md and
    docs/INDEX.md first.

    Template origin: https://github.com/edrethardo/developer_agent — replace with the
    local template folder's absolute path if one is kept on this machine.

    Tool repos on this machine (list may be stale — verify a path exists before use):
    - <tool name> — <absolute path>
    <!-- developer-agent:end -->

### 7. Close out

- Write the first journal entry — file `docs/journal/YYYY-MM-DD-init.md` — per the
  `journaling` skill, documenting this init: answers given, versions installed, anything
  skipped. Skip if a `*-init.md` entry already exists.
- Check every preceding step really happened before going further: `docs/user/about.md`
  exists, no `<… set at init>` markers remain, the CHANGELOG placeholder is gone,
  template memoirs are pruned, all six skills exist in `~/.claude/skills/` at the
  shipped version or newer, and the `~/.claude/CLAUDE.md` block lists this tool. If any
  of these is not true, STOP here: keep the marker, list the pending items for the user
  in plain language, and say init will resume next conversation.
- Only if all of them hold: as a separate final edit (never combined with the index-line
  edit above), remove the `<!-- UNINITIALIZED -->` line from `docs/journal/INDEX.md` —
  the LAST file edit of init.
- Commit: `Initialize <tool name>` (skip if the tree is clean).
- Only now suggest renaming the folder to the tool's name: close VSCode, rename in the
  file manager, reopen. Skippable. If they rename, the path line in `~/.claude/CLAUDE.md`
  goes stale — acceptable by design: the block itself warns that paths may be stale and
  must be verified before use. An agent must not rename its own working directory.
- Hand over: "Setup's done. Tell me what you'd like the tool to do first."
