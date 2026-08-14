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

### 2. Interview — one question per message

Ask and remember the answers — the file writes all happen in step 3. Skip any question
whose answer is already recorded in CLAUDE.md.

a. Preferred language.
b. What should this tool do? A short paragraph is enough — details come per-feature later.
c. What should the tool be called?

### 3. Mechanical setup (narrate briefly as you go)

Done-checks in parentheses; skip satisfied steps:

- Git repo exists (`.git/` present): else `git init -b main`.
- Git identity works (`git config user.name` prints something): else set it REPO-LOCALLY
  — `git config user.name "<tool name>"` and `git config user.email
  "<tool-slug>@local.invalid"`. Never `--global`; the user is not asked.
- Pristine baseline committed (`git log` shows at least one commit): else commit
  everything as "Initial commit: pristine template" BEFORE any personalization edit.
- CLAUDE.md personalized (no `<… set at init>` markers remain): write tool name, one-line
  description, user language.
- `docs/user/about.md` exists: write it in the user's language — what this tool is for,
  from the interview answer. Frontmatter per the `documenting` skill; add its line to
  `docs/INDEX.md`.
- The setup placeholder under `## [Unreleased]` in CHANGELOG.md is gone (and the header
  is in the user's language): rewrite the header paragraph if needed, and replace the
  placeholder line with a first entry saying the tool was set up today.
- Commit the personalization: `Personalize template for <tool name>`.

### 4. Install shared skills

For each folder in `.claude/skills/_user-level/`, target `~/.claude/skills/<name>/`:

- Not present there → copy it by READING each file and WRITING it to the target with the
  file tools (pre-approved paths); never shell out to `cp`.
- Present → compare the integer `version:` in both SKILL.md frontmatters; copy (the same
  way) only if the shipped version is GREATER. Never downgrade. Never touch skills this
  template didn't ship.

### 5. Machine-wide CLAUDE.md block

File: `~/.claude/CLAUDE.md` — create it if missing. NEVER modify anything outside the
delimiters. Rules:

- No `<!-- developer-agent:start` block → append the block below to the end of the file,
  with the `- <tool name> — <absolute path>` placeholder line REPLACED by this tool's
  real entry.
- Block exists with a LOWER version in the start delimiter → replace the block's content
  (delimiter to delimiter) with the current template, preserving the existing tool list.
- Block exists with an EQUAL or HIGHER version → leave its content alone.
- Then ensure THIS tool is listed exactly once, keyed by absolute path: path already
  listed → leave it; otherwise append one line `- <tool name> — <absolute path>`.

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

Skip if the init journal entry already exists (this ran before). One short paragraph,
user's language, own message: routine work runs without asking; unusual actions show a
permission box; a box means "stop and consider" — the user can always ask "what does
this mean?" and get a plain answer first. Adding something to the always-allowed list is
possible, and the worst case gets explained before it happens (CLAUDE.md hard rule 8).

### 7. Close out

- Write the first journal entry (per the `journaling` skill) documenting this init:
  answers given, versions installed, anything skipped. Skip if it already exists.
- Remove the `<!-- UNINITIALIZED -->` line from `docs/journal/INDEX.md` — LAST file edit
  of init.
- Commit: `Initialize <tool name>`.
- Only now suggest renaming the folder to the tool's name: close VSCode, rename in the
  file manager, reopen. Skippable. If they do it, correct the path line in
  `~/.claude/CLAUDE.md` next session (the block itself warns it may be stale). An agent
  must not rename its own working directory.
- Hand over: "Setup's done. Tell me what you'd like the tool to do first."
