---
name: initialize-tool
description: Use when docs/journal/INDEX.md contains the UNINITIALIZED marker — first-run setup; interview the user, set up git, install shared skills, configure the machine.
version: 1
---

# Initialize Tool

First-run setup. Entirely conversational — the user never opens a terminal. Reply in the
language of the user's first message throughout.

Every step is idempotent: each has a done-check and is skipped when already satisfied, so
an interrupted init simply resumes next conversation. The `<!-- UNINITIALIZED -->` marker
in `docs/journal/INDEX.md` is removed LAST — marker present means init is incomplete.

## Steps, in order

### 1. Prerequisites

Detect the OS from the environment. Check git: run `git --version`.
If git is missing: explain in plain words what git is (the change-history system this
setup depends on) and guide the user through installing it for THEIR OS — this genuinely
needs their clicks on an installer; say so honestly. Do not continue without git.

### 2. Interview — one question per message

Skip any question whose answer is already recorded in CLAUDE.md.

a. Preferred language → write into the CLAUDE.md "User language" line.
b. What should this tool do? A short paragraph is enough — details come per-feature later.
c. What should the tool be called?

### 3. Mechanical setup (narrate briefly as you go)

Done-checks in parentheses; skip satisfied steps:

- Git repo (`.git/` exists): else `git init -b main`, then commit everything as
  "Initial commit: pristine template".
- CLAUDE.md personalized (no `<… set at init>` markers remain): write tool name, one-line
  description, user language.
- `docs/user/about.md` exists: write it in the user's language — what this tool is for,
  from the interview answer. Frontmatter per the `documenting` skill; add its line to
  `docs/INDEX.md`.
- CHANGELOG.md header is in the user's language: rewrite the header paragraph only.
- Folder name: suggest the user rename the folder to the tool's name in their file
  manager — the ONE manual, skippable step (an agent must not rename its own working
  directory). Offer to continue regardless.

### 4. Install shared skills

For each folder in `.claude/skills/_user-level/`, target `~/.claude/skills/<name>/`:

- Not present there → copy the folder.
- Present → compare the integer `version:` in both SKILL.md frontmatters; copy only if
  the shipped version is GREATER. Never downgrade. Never touch skills this template
  didn't ship.

### 5. Machine-wide CLAUDE.md block

File: `~/.claude/CLAUDE.md` — create it if missing. NEVER modify anything outside the
delimiters. Rules:

- No `<!-- developer-agent:start` block → append the block below to the end of the file.
- Block exists with a lower version in the start delimiter → replace the block's content
  (delimiter to delimiter) with the current block, preserving the existing tool list.
- Then ensure THIS tool (name — absolute path) is one line in the block's tool list.

Block template (verbatim):

    <!-- developer-agent:start v1 -->
    ## Developer-agent tool repos

    Shared skills for these repos live in ~/.claude/skills/ (journaling,
    finding-knowledge, git-discipline, explaining-complexity, documenting,
    creating-skills) — use them. Before investigating anything in one of these repos,
    read its docs/journal/INDEX.md and docs/INDEX.md first.

    Tool repos on this machine (list may be stale — verify a path exists before use):
    - <tool name> — <absolute path>
    <!-- developer-agent:end -->

### 6. Permissions explainer

One short paragraph, user's language, own message: routine work runs without asking;
unusual actions show a permission box; a box means "stop and consider" — the user can
always ask "what does this mean?" and get a plain answer first. Adding something to the
always-allowed list is possible, and the worst case gets explained before it happens
(CLAUDE.md hard rule 8).

### 7. Close out

- Write the first journal entry (per the `journaling` skill) documenting this init:
  answers given, versions installed, anything skipped.
- Remove the `<!-- UNINITIALIZED -->` line from `docs/journal/INDEX.md` — LAST file edit
  of init.
- Commit: `Initialize <tool name>`.
- Hand over: "Setup's done. Tell me what you'd like the tool to do first."
