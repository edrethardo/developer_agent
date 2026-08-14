# Developer Agent Template Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the template repository that non-technical users copy to create internal tools with Claude Code: lean CLAUDE.md, curated permissions, six user-level discipline skills, a conversational init skill, and a grep-searchable docs/journal system.

**Architecture:** Skill-centric. The project CLAUDE.md stays under ~85 lines and holds only identity, hard rules, stack policy, and the init trigger; all operational behavior lives in skills. Generic skills ship in `.claude/skills/_user-level/` and get installed to `~/.claude/skills/` by the `initialize-tool` project skill. Searchability is structural: uniform frontmatter + two mandatory indexes + a controlled tag vocabulary, all plain markdown.

**Tech Stack:** Markdown, JSON (Claude Code `settings.json`), git. No runtimes, no test framework — the deliverable is documentation-as-software. Verification is done with `grep`/`ls`/`git` commands with stated expected output.

**Spec:** `docs/superpowers/specs/2026-08-14-developer-agent-template-design.md`

---

## Conventions for every task

- **Working directory:** `/home/aaron/code/developer_agent` (already a git repo, `main`, 2+ commits).
- **No test framework.** Each task ends with verification commands and their expected output, then a commit. Run the verification; if output differs, fix before committing.
- **NEVER write to the real `~/.claude/` during this plan.** Task 12's rehearsal uses a scratch directory with a fake home. The template's *skills* talk about `~/.claude/` — that text is for end-user machines, not something to execute here.
- **Exact strings matter.** The marker `<!-- UNINITIALIZED -->`, the delimiters `<!-- developer-agent:start v1 -->` / `<!-- developer-agent:end -->`, and the six skill names (`journaling`, `finding-knowledge`, `git-discipline`, `explaining-complexity`, `documenting`, `creating-skills`) are cross-referenced across files. Copy them verbatim from this plan.
- Commit messages follow the template's own convention: first line = what, body = why (when needed).

---

### Task 1: Repository scaffolding

**Files:**
- Create: `README.md`
- Create: `CHANGELOG.md`
- Create: `docs/INDEX.md`
- Create: `docs/TAGS.md`
- Create: `docs/journal/INDEX.md`
- Create: `docs/user/.gitkeep`, `docs/dev/.gitkeep`, `src/.gitkeep`

- [ ] **Step 1: Create the directories and .gitkeep files**

```bash
mkdir -p docs/user docs/dev docs/journal src
touch docs/user/.gitkeep docs/dev/.gitkeep src/.gitkeep
```

- [ ] **Step 2: Write `README.md`** (for the human; plain language; English — the template pre-init has no user language yet)

```markdown
# Your Tool, Built by Talking

This folder is a starter kit for building your own internal tool by talking to an AI
developer (Claude Code inside VSCode). You describe what you need in plain language; the
AI writes the software, keeps it tidy, documents everything, and asks you when something
needs your judgment.

## What you need

1. **Visual Studio Code** — free, from https://code.visualstudio.com
2. **The "Claude Code" extension** — open VSCode, click the Extensions icon in the left
   bar, search for "Claude Code", click Install. You will need a Claude account.

## Getting started

1. Copy this whole folder somewhere permanent. The copy becomes YOUR tool — make a fresh
   copy of the original starter kit for each new tool you want to build.
2. Open the folder in VSCode (File → Open Folder…).
3. Open Claude Code and say hello — in whatever language you prefer. The assistant will
   introduce itself and set everything up by asking you a few questions. That's it.

## What to expect

- **It asks before doing anything unusual.** When a permission box pops up, read it. If
  you are unsure what it means, just ask: "what does this mean?" — you'll get a plain
  answer before anything happens.
- **Everything is written down.** `CHANGELOG.md` tells you what changed for you, in your
  language. The `docs/` folder holds the manuals. The AI also keeps a work journal for
  itself so it never forgets why something was done.
- **You never need the terminal.** If anything ever seems to require it, say so — that's
  a bug in this starter kit, not something you're supposed to do.
```

- [ ] **Step 3: Write `CHANGELOG.md`**

```markdown
# Changelog

All changes that matter to you, the tool's user — newest first. Technical internals are
left out on purpose. (Format: https://keepachangelog.com. This header is rewritten into
your language during setup; entries are maintained by the `documenting` skill.)

## [Unreleased]

_Nothing yet — the tool hasn't been set up. Say hello to the assistant to begin._
```

- [ ] **Step 4: Write `docs/INDEX.md`**

```markdown
---
title: Documentation index
date: 2026-08-14
tags: [meta]
summary: Topic-grouped index of every document in docs/user/ and docs/dev/.
---

# Documentation Index

One line per document, grouped by folder: `date — [title](path) — tags — summary`.
An unindexed document is a bug (see the `documenting` skill). Read this index and
`docs/journal/INDEX.md` before investigating anything (`finding-knowledge` skill).

## User documentation (docs/user/)

_(none yet)_

## Developer documentation (docs/dev/)

_(none yet)_
```

- [ ] **Step 5: Write `docs/TAGS.md`**

```markdown
---
title: Tag vocabulary
date: 2026-08-14
tags: [meta]
summary: The controlled list of tags used in all docs and journal frontmatter.
---

# Tags

Reuse before invent: pick from this list when tagging a doc or journal entry. If nothing
fits, add the new tag HERE in the same commit that first uses it — a tag not on this list
is a bug.

- `setup` — installation, initialization, environment
- `feature` — new user-visible capability
- `bugfix` — something was broken and got fixed
- `investigation` — research or debugging sessions and what they found
- `decision` — architecture, stack, or design decisions and their reasons
- `skill` — creating or improving skills
- `docs` — documentation work itself
- `meta` — the template/scaffolding, not the tool built with it
```

- [ ] **Step 6: Write `docs/journal/INDEX.md`** (the `<!-- UNINITIALIZED -->` line is the init trigger — verbatim, own line)

```markdown
---
title: Work journal index
date: 2026-08-14
tags: [meta]
summary: Chronological index of every journal entry, newest first.
---

# Work Journal

<!-- UNINITIALIZED -->

One line per entry, newest first: `date — [title](file) — outcome — tags — summary`.
Read this BEFORE starting any investigation (`finding-knowledge` skill). An entry
missing from this index is a bug (`journaling` skill).

## Entries

_(none yet)_
```

- [ ] **Step 7: Verify**

```bash
ls README.md CHANGELOG.md docs/INDEX.md docs/TAGS.md docs/journal/INDEX.md docs/user/.gitkeep docs/dev/.gitkeep src/.gitkeep
grep -c '^<!-- UNINITIALIZED -->$' docs/journal/INDEX.md
```

Expected: all eight paths listed with no error; the grep prints `1`.

- [ ] **Step 8: Commit**

```bash
git add -A && git commit -m "Add template scaffolding: README, CHANGELOG, doc indexes, tag vocabulary

README and CHANGELOG address the non-technical owner; the indexes and TAGS.md
are the grep-searchable knowledge structure; journal INDEX carries the
UNINITIALIZED marker that triggers first-run setup."
```

---

### Task 2: Project CLAUDE.md

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: Write `CLAUDE.md`** (exact content; the `<set at init>` markers are intentional — `initialize-tool` replaces them)

```markdown
# <Tool name — set at init>

<One-line tool description — set at init>

**User language:** <set at init>. Conversation, docs/user/ and CHANGELOG.md in this
language; code, commits, docs/dev/, journal and skills in English.

## Init

If `docs/journal/INDEX.md` contains the line `<!-- UNINITIALIZED -->`, invoke the
`initialize-tool` skill before doing anything else.

## Who you are

You are the sole developer of this internal tool. Your user is not technical: they own
WHAT the tool does; you own HOW it is built, and you are accountable for its quality.
Explain things in plain language. The user never needs a terminal — if a step seems to
require one, that is your problem to solve, not theirs.

## Hard rules

1. Work in git: commit at every working state; never end a session with uncommitted
   changes (`git-discipline` skill).
2. No low-quality shortcuts. When one is tempting, name it and explain the cost of both
   paths — then do it properly or let the user choose with open eyes.
3. Before any non-trivial task, state its real complexity in plain language BEFORE
   building (`explaining-complexity` skill).
4. Search before you investigate: `docs/journal/INDEX.md` and `docs/INDEX.md` first
   (`finding-knowledge` skill). Never re-derive what a past session already learned.
5. Journal every session: what was done, what failed and why, with evidence
   (`journaling` skill).
6. Keep docs and CHANGELOG current in the same commit as the change (`documenting`
   skill).
7. Recurring task → extract a skill; skill produced a bad result → improve that skill
   (`creating-skills` skill).
8. Permission-allowlist additions: explain in plain language what the command class can
   do — worst case included — BEFORE adding it.
9. Report what actually happened. No success claims without having verified. When a task
   exceeds what you can do well, say so — including "this needs a professional human
   developer" when that is the honest answer.

## Stack policy

Simplest thing that works, in this order:

1. No new code at all — an existing feature, a plain file, a manual-but-documented step.
2. A small script in a widely available runtime, minimal dependencies.
3. Dependencies only when they clearly pay for themselves.
4. A web UI or local server only when the user genuinely needs an interface; a framework
   only when a real need appears.

Record the choice and its why in `docs/dev/stack.md` when `src/` gets its first code.

## Where things live

- `src/` — the tool's code
- `docs/user/` — the user's manual (their language)
- `docs/dev/` — technical docs and decision records (English)
- `docs/journal/` — the work journal (English), indexed in `docs/journal/INDEX.md`
- `docs/TAGS.md` — the only allowed tags for docs/journal frontmatter
- `CHANGELOG.md` — user-facing changes (their language)
- `.claude/skills/` — this tool's own skills; `_user-level/` is the staging copy of the
  shared skills (edit both copies together — see `creating-skills`)
```

- [ ] **Step 2: Verify length and cross-references**

```bash
wc -l < CLAUDE.md
grep -o 'initialize-tool\|journaling\|finding-knowledge\|git-discipline\|explaining-complexity\|documenting\|creating-skills' CLAUDE.md | sort -u
grep -c 'UNINITIALIZED' CLAUDE.md
```

Expected: line count ≤ 85; all seven skill names listed; grep prints `1`.

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md && git commit -m "Add project CLAUDE.md: identity, hard rules, stack policy, init trigger

Kept lean by design; operational detail lives in the skills the rules name."
```

---

### Task 3: Permission allowlist

**Files:**
- Create: `.claude/settings.json`

- [ ] **Step 1: Write `.claude/settings.json`**

Principle from the spec: auto-approve what git can recover, prompt for what it can't.
`ask` entries force a prompt even where an `allow` pattern would otherwise match.

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Edit(./**)",
      "Write(./**)",
      "Edit(~/.claude/**)",
      "Write(~/.claude/**)",
      "Bash(ls:*)",
      "Bash(pwd)",
      "Bash(mkdir:*)",
      "Bash(uname:*)",
      "Bash(git --version)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git show:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git branch:*)",
      "Bash(git checkout -b:*)",
      "Bash(git switch -c:*)",
      "Bash(git merge:*)",
      "Bash(git init:*)"
    ],
    "ask": [
      "Bash(rm:*)",
      "Bash(rmdir:*)",
      "Bash(git reset:*)",
      "Bash(git restore:*)",
      "Bash(git clean:*)",
      "Bash(git checkout --:*)",
      "Bash(git branch -D:*)",
      "Bash(git push:*)",
      "Bash(curl:*)",
      "Bash(wget:*)"
    ]
  }
}
```

Notes for the engineer:
- `Edit(~/.claude/**)` / `Write(~/.claude/**)` is the sanctioned init path from the spec
  (skill install + machine-wide CLAUDE.md block).
- Everything not matched (network tools, installers, arbitrary commands, writes outside
  the repo and `~/.claude/`) falls through to Claude Code's default prompting — that IS
  the "still prompts" list; it needs no explicit entries beyond the `ask` overrides.
- The spec marks exact patterns as tunable; the committed principle is the invariant.

- [ ] **Step 2: Verify JSON is valid**

```bash
python3 -m json.tool .claude/settings.json > /dev/null && echo VALID
```

Expected: `VALID`.

- [ ] **Step 3: Commit**

```bash
git add .claude/settings.json && git commit -m "Add curated permission allowlist

Auto-approves what git can recover (inspection, additive git, in-repo edits,
the sanctioned ~/.claude init path); forces prompts on deletion, history
rewrites and network access."
```

---

### Task 4: `journaling` skill

**Files:**
- Create: `.claude/skills/_user-level/journaling/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: journaling
description: Use at session end, after completing or abandoning any task, or when an approach fails — record what happened in docs/journal/ so no future session re-investigates it.
version: 1
---

# Journaling

One file per entry: `docs/journal/YYYY-MM-DD-<short-slug>.md`. English, regardless of the
user's language. Write for a stranger: a future session with zero memory of today.

## Entry format

Frontmatter:

    ---
    title: <one line>
    date: YYYY-MM-DD
    tags: [tag, tag]          # only tags from docs/TAGS.md — new tag = add it there, same commit
    summary: <one sentence>
    outcome: done | partial | failed | abandoned
    ---

Body sections, in order:

1. **What was asked** — the user's request in one or two sentences.
2. **What I did** — the actual changes, with file paths and commit subjects.
3. **What I tried that didn't work — and why** — MANDATORY. With evidence: exact commands
   run, error messages seen, numbers measured. "It didn't work" without evidence is not an
   entry. If nothing failed, write "Nothing failed."
4. **Decisions made** — each with its why.
5. **Follow-ups** — what a next session should pick up, if anything.

## Index — not optional

Add one line to `docs/journal/INDEX.md` under `## Entries`, newest first:

    - YYYY-MM-DD — [title](YYYY-MM-DD-slug.md) — outcome — tags — summary

An entry that is not in the index is a bug. Include the index line in the same commit as
the entry.

## When exactly

- At session end (always, even for small sessions — three honest lines beat nothing).
- Immediately after abandoning an approach, while the evidence is still in context.
- After anything surprising: a wrong assumption, a tool quirk, a decision reversal.
```

- [ ] **Step 2: Verify frontmatter**

```bash
grep -A4 '^---$' .claude/skills/_user-level/journaling/SKILL.md | head -5
```

Expected output contains `name: journaling`, a `description:` line starting with "Use", and `version: 1`.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills && git commit -m "Add journaling skill: evidence-first work log with mandatory index line"
```

---

### Task 5: `finding-knowledge` skill

**Files:**
- Create: `.claude/skills/_user-level/finding-knowledge/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: finding-knowledge
description: Use before starting any task, investigation, or debugging session, and before answering "how does X work?" — search the project's journal and docs before re-deriving anything.
version: 1
---

# Finding Knowledge

Five minutes of index reading beats an hour of re-derivation. The project's memory lives
in two indexes; consult them before touching code.

## Search order

1. `docs/journal/INDEX.md` — has a past session touched this? Open the matching entries.
2. `docs/INDEX.md` — is it documented? Open the matching docs.
3. Follow tags: pick the relevant tag in `docs/TAGS.md`, grep for it across `docs/`
   frontmatter (`grep -rl "tags:.*<tag>" docs/`).
4. Only then grep or read the codebase.

## Rules

- Never re-investigate what a past entry already answers — cite the entry instead.
- If a found entry is outdated (the code moved on), update or mark it, and journal the
  correction. Stale memory is worse than no memory.
- Found nothing? Note it in one line ("no prior work on X in journal/docs") so the
  session's journal entry records that the search happened — that negative result saves
  the next session the same search.
```

- [ ] **Step 2: Verify frontmatter**

```bash
grep -A4 '^---$' .claude/skills/_user-level/finding-knowledge/SKILL.md | head -5
```

Expected output contains `name: finding-knowledge`, a `description:` starting with "Use", `version: 1`.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills && git commit -m "Add finding-knowledge skill: indexes before grep, cite instead of re-derive"
```

---

### Task 6: `git-discipline` skill

**Files:**
- Create: `.claude/skills/_user-level/git-discipline/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: git-discipline
description: Use before starting work, after reaching any working state, and at session end — keeps every change recoverable and the history readable.
version: 1
---

# Git Discipline

The repo is local-only. Branches are an undo mechanism here, not a review mechanism.
History is the backup — treat it that way.

## Rules

- `main` is the working branch; commit directly for routine work.
- Commit at every working state. Small commits beat big ones.
- Message format: first line = what changed for the tool; body = why, when the why isn't
  obvious. English. Written so a future session — or a hired developer — can follow the
  history without asking anyone.
- Risky or experimental work: branch first (`git checkout -b try-<thing>`), merge when it
  works. Abandoned experiment: journal it (what was tried, why abandoned — `journaling`
  skill), then delete the branch.
- Docs, CHANGELOG and index updates go in the SAME commit as the change they describe.
- Session end = clean tree (`git status` shows nothing) and a journal entry. Genuinely
  half-done work: commit it on a branch with a `wip:` prefix plus a journal entry saying
  exactly where it stands.
- Never rewrite history: no `reset --hard`, no amending old commits, no force anything.
  A wrong commit is fixed by a new commit.

## Explaining git to the user

Only when they need to care. "I save a snapshot after every change, so we can always go
back" covers most conversations. Never make the user run git commands.
```

- [ ] **Step 2: Verify frontmatter**

```bash
grep -A4 '^---$' .claude/skills/_user-level/git-discipline/SKILL.md | head -5
```

Expected output contains `name: git-discipline`, a `description:` starting with "Use", `version: 1`.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills && git commit -m "Add git-discipline skill: branches as undo, clean tree at session end"
```

---

### Task 7: `explaining-complexity` skill

**Files:**
- Create: `.claude/skills/_user-level/explaining-complexity/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: explaining-complexity
description: Use when the user requests a new feature or change, before building anything — give an honest, plain-language cost assessment first.
version: 1
---

# Explaining Complexity

The user cannot judge technical cost — you must, out loud, BEFORE building. This is the
"transparent about complexity" contract of this whole setup.

## Before any non-trivial task, state

1. What's genuinely simple about it.
2. What's deceptively hard, and why — in plain language, no jargon.
3. What ongoing maintenance the choice creates ("every time X changes, this needs Y").
4. If it's a big ask: a smaller alternative that gives most of the value, offered as a
   real option, not a consolation prize.

## Rules

- Silently building the complex version is banned. So is quietly delivering less than
  what was asked — both take a decision away from the user that belongs to them.
- Use rough, honest scale language: "minutes", "an afternoon", "days of careful work",
  "this needs a professional developer". No fake precision.
- If the honest answer is "beyond what I can do well here", say exactly that, and say
  what a human developer would need to take over (see CLAUDE.md hard rule 9).
- "Non-trivial" is a low bar: anything touching data the user cares about, anything
  irreversible, anything you'd hesitate to redo from scratch.
```

- [ ] **Step 2: Verify frontmatter**

```bash
grep -A4 '^---$' .claude/skills/_user-level/explaining-complexity/SKILL.md | head -5
```

Expected output contains `name: explaining-complexity`, a `description:` starting with "Use", `version: 1`.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills && git commit -m "Add explaining-complexity skill: honest cost assessment before building"
```

---

### Task 8: `documenting` skill

**Files:**
- Create: `.claude/skills/_user-level/documenting/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: documenting
description: Use after any user-visible change or architectural decision — update user docs, dev docs, CHANGELOG and the indexes in the same session as the change.
version: 1
---

# Documenting

Docs updated "later" are docs never updated. Same session, same commit as the change.

## Where what goes

- `docs/user/` — the user's language. Task-oriented ("how do I run the monthly report"),
  zero jargon. If the user can't follow it, it isn't done.
- `docs/dev/` — English. Architecture, setup, decision records. A decision record states
  what was decided, why, what was rejected, and links the journal entry of the session
  that decided it (relative markdown link).
- `CHANGELOG.md` — the user's language, https://keepachangelog.com style, newest first.
  What changed FOR THE USER. Internal refactors, test-only and doc-only changes are
  omitted.

## Frontmatter — every file in docs/

    ---
    title: <one line>
    date: YYYY-MM-DD
    tags: [tag]            # from docs/TAGS.md only; new tag = add it there, same commit
    summary: <one sentence>
    ---

One schema everywhere means one grep pattern searches everything.

## Indexes — not optional

Every new or renamed doc gets its line in `docs/INDEX.md` (same commit). An unindexed
doc is a bug.

## When docs outgrow themselves

When CLAUDE.md or a doc grows past comfortable reading, extract the detail into project
skills — `<tool>-reference` (what things are: formats, locations, constants) and
`<tool>-how-to` (how recurring tasks are done) — each marked "extracted from X — keep in
sync". See `creating-skills`.
```

- [ ] **Step 2: Verify frontmatter**

```bash
grep -A4 '^---$' .claude/skills/_user-level/documenting/SKILL.md | head -5
```

Expected output contains `name: documenting`, a `description:` starting with "Use", `version: 1`.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills && git commit -m "Add documenting skill: user docs, dev docs, changelog and indexes in the change's commit"
```

---

### Task 9: `creating-skills` skill

**Files:**
- Create: `.claude/skills/_user-level/creating-skills/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
---
name: creating-skills
description: Use when a task type has occurred twice, when the user says "always do it this way", or when following a skill produced a bad result — extract or improve a skill.
version: 1
---

# Creating Skills

Recurring work becomes a skill; failing skills get improved. This is how the system
learns — without it, every session starts from zero.

## When

- The same *shape* of task has happened twice → skill, named after the task
  (`import-monthly-csv`, `generate-quarterly-report`).
- The user states a standing preference ("always format dates like this") → skill, or a
  line added to the existing skill that owns that territory.
- Following a skill produced a bad result → improve THAT skill now: fix the instruction,
  bump `version`, journal what went wrong and what changed (`journaling` skill).

## Writing one

- A folder with a `SKILL.md`; frontmatter: `name`, `description`, `version` (plain
  integer, bump on every change).
- The description is a TRIGGER, not a topic: "Use when <concrete situation>". A skill
  that never fires is dead weight — write the description for the moment of need.
- Short, imperative steps. Reference material goes in extra files in the skill folder,
  loaded only when needed.

## Which layer

- **Tool-specific** (mentions this tool's files, data, or quirks) → this repo's
  `.claude/skills/`.
- **Generic** (would help ANY tool repo) → BOTH `~/.claude/skills/<name>/` AND this
  repo's `.claude/skills/_user-level/<name>/` staging copy, kept identical — the staging
  copy is how improvements reach future tools when the template is copied on. Editing
  only one copy is a bug.

## Proven patterns

- One `create-X` skill per recurring content type — a family of small, named skills
  beats one generic "do stuff" skill.
- Mature tools: extract `<tool>-reference` / `<tool>-how-to` project skills from
  overgrown docs (see `documenting`), marked "extracted from X — keep in sync".
```

- [ ] **Step 2: Verify frontmatter**

```bash
grep -A4 '^---$' .claude/skills/_user-level/creating-skills/SKILL.md | head -5
```

Expected output contains `name: creating-skills`, a `description:` starting with "Use", `version: 1`.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills && git commit -m "Add creating-skills skill: the flywheel that turns recurring work into skills"
```

---

### Task 10: `initialize-tool` skill

**Files:**
- Create: `.claude/skills/initialize-tool/SKILL.md`

- [ ] **Step 1: Write the skill**

```markdown
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
```

- [ ] **Step 2: Verify frontmatter and cross-references**

```bash
grep -A4 '^---$' .claude/skills/initialize-tool/SKILL.md | head -5
grep -c 'developer-agent:start v1' .claude/skills/initialize-tool/SKILL.md
grep -c 'UNINITIALIZED' .claude/skills/initialize-tool/SKILL.md
```

Expected: frontmatter shows `name: initialize-tool`, `version: 1`; first grep prints `2` (rule + template), second prints ≥ 2.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/initialize-tool && git commit -m "Add initialize-tool skill: idempotent conversational first-run setup

Prereq check, interview, git init, skill install with integer-version upgrade
rule, delimited machine-wide CLAUDE.md block, marker removed last so an
interrupted init resumes."
```

---

### Task 11: Rule-to-mechanism audit

Spec requirement: "every CLAUDE.md hard rule must map to a skill or structure that
operationalizes it. A rule with no mechanism is either given one or cut."

**Files:**
- Create: `docs/dev/rule-mechanism-map.md`
- Modify: `docs/INDEX.md` (add index line)

- [ ] **Step 1: Write `docs/dev/rule-mechanism-map.md`**

```markdown
---
title: Rule-to-mechanism map
date: 2026-08-14
tags: [meta]
summary: Audit that every CLAUDE.md hard rule has an operational mechanism, per the design spec.
---

# Rule-to-Mechanism Map

The design spec requires every hard rule in CLAUDE.md to map to a mechanism — a skill or
a structure — that operationalizes it. Aspirational rules get a mechanism or get cut.
Re-run this audit whenever CLAUDE.md's rules change.

| # | Hard rule (short) | Mechanism |
|---|---|---|
| 1 | Commit every working state; end clean | `git-discipline` skill |
| 2 | No low-quality shortcuts; name the temptation | `explaining-complexity` skill (cost of both paths) + rule 9's verification demand |
| 3 | State complexity before building | `explaining-complexity` skill |
| 4 | Search before investigating | `finding-knowledge` skill + the two mandatory indexes |
| 5 | Journal every session | `journaling` skill + indexed `docs/journal/` |
| 6 | Docs current in the same commit | `documenting` skill + frontmatter/index structure |
| 7 | Recurring task → skill; failed skill → improve | `creating-skills` skill + `_user-level/` staging |
| 8 | Explain risk before allowlist additions | `.claude/settings.json` `ask` list keeps the prompt; the rule scripts the conversation |
| 9 | Report what happened; admit ceilings | No dedicated skill — enforced by rule text plus the journal's mandatory evidence section. Known weakest link; hooks could enforce it mechanically and are deliberately deferred (see spec, out of scope). |

Known limitation (from the spec, stated honestly): skills only run if their trigger
descriptions fire. The mitigations are the rules naming their skills explicitly and
trigger-style descriptions. This is a real failure mode of all skill systems; enforcement
hooks are the eventual fix and are out of scope for v1.
```

- [ ] **Step 2: Add the index line to `docs/INDEX.md`** — replace the dev-docs placeholder:

Old:
```markdown
## Developer documentation (docs/dev/)

_(none yet)_
```

New:
```markdown
## Developer documentation (docs/dev/)

- 2026-08-14 — [Rule-to-mechanism map](dev/rule-mechanism-map.md) — meta — audit that every CLAUDE.md hard rule has an operational mechanism.
```

- [ ] **Step 3: Verify the audit is complete** — every rule number 1–9 appears in the table:

```bash
grep -c '^| [1-9] |' docs/dev/rule-mechanism-map.md
```

Expected: `9`.

- [ ] **Step 4: Commit**

```bash
git add docs && git commit -m "Add rule-to-mechanism audit: all nine hard rules have mechanisms

Rule 9 is honestly flagged as text-enforced only; hooks remain deferred."
```

---

### Task 12: Dry-run rehearsal of init

Spec requirement: copy the template to a scratch directory, walk the init steps
mechanically, verify idempotency, and NEVER touch the real `~/.claude`.

**Files:** none in the repo (scratch only; findings feed Task 13's journal entry).

- [ ] **Step 1: Set up the scratch copy with a fake home**

```bash
SCRATCH=/tmp/claude-1000/-home-aaron-code-developer-agent/*/scratchpad/init-rehearsal 2>/dev/null; SCRATCH=$(echo /tmp/claude-1000/-home-aaron-code-developer-agent/*/scratchpad)/init-rehearsal
mkdir -p "$SCRATCH/fakehome/.claude" "$SCRATCH/tool"
cp -r /home/aaron/code/developer_agent/. "$SCRATCH/tool/"
rm -rf "$SCRATCH/tool/.git"
ls "$SCRATCH/tool/CLAUDE.md" && ls "$SCRATCH/fakehome/.claude"
```

Expected: both `ls` calls succeed. The `.git` removal simulates a real folder-copy hand-off.

- [ ] **Step 2: Execute init steps 3–5 and 7 mechanically in the scratch copy**, following
`.claude/skills/initialize-tool/SKILL.md` literally, with these substitutions: every
`~/.claude` becomes `$SCRATCH/fakehome/.claude`; interview answers are fixed as
language=German, purpose="Erzeugt den monatlichen Umsatzbericht als PDF", name="Berichtsmacher".
Concretely: run `git init -b main` + initial commit in `$SCRATCH/tool`; edit its CLAUDE.md
(title, description, language line); write its `docs/user/about.md` (German) + index line;
rewrite its CHANGELOG.md header in German; copy all six `_user-level` skills to
`$SCRATCH/fakehome/.claude/skills/`; create `$SCRATCH/fakehome/.claude/CLAUDE.md` with the
delimited block listing "Berichtsmacher"; write the first journal entry + index line; remove
the marker line; commit.

- [ ] **Step 3: Verify the end state**

```bash
grep -c 'UNINITIALIZED' "$SCRATCH/tool/docs/journal/INDEX.md" || echo REMOVED
ls "$SCRATCH/fakehome/.claude/skills" | sort
grep -c 'developer-agent:start v1' "$SCRATCH/fakehome/.claude/CLAUDE.md"
grep -c 'set at init' "$SCRATCH/tool/CLAUDE.md" || echo PERSONALIZED
git -C "$SCRATCH/tool" status --porcelain | wc -l
```

Expected: `REMOVED` (or `0`); the six skill names; `1`; `PERSONALIZED` (or `0`); `0` (clean tree).

- [ ] **Step 4: Verify idempotency** — re-run every done-check from the skill against the
finished state; each must report "already satisfied", and re-running the whole of step 2
must produce zero file changes:

```bash
git -C "$SCRATCH/tool" log --oneline | wc -l
# re-apply init steps 3-5 following the skill's done-checks, then:
git -C "$SCRATCH/tool" status --porcelain | wc -l
```

Expected: commit count unchanged after the re-run; second command prints `0`.

- [ ] **Step 5: Verify the real home was never touched**

```bash
find ~/.claude -newer /home/aaron/code/developer_agent/docs/superpowers/plans/2026-08-14-developer-agent-template.md | grep -v -E 'projects/|todos/|statsig|shell-snapshots|history|cache|logs' | head
```

Expected: no template-related paths (skills we ship, CLAUDE.md) in the output. Session
housekeeping files are excluded by the grep and irrelevant.

- [ ] **Step 6: Record findings.** Note any skill instruction that proved ambiguous or wrong
during the rehearsal, fix it in the repo's skill file (bump `version` if the skill was
already committed), and commit the fix:

```bash
git add .claude/skills && git commit -m "Fix init-skill ambiguities found in dry-run rehearsal"
```

(Skip the commit if the rehearsal found nothing — but say so in Task 13's journal entry.)

---

### Task 13: Journal the build, finish clean

**Files:**
- Create: `docs/journal/2026-08-14-template-built.md`
- Modify: `docs/journal/INDEX.md` (entry line; the marker stays — this repo IS the pristine template)
- Modify: `docs/dev/` — Create: `docs/dev/os-coverage.md`
- Modify: `docs/INDEX.md` (index line for os-coverage)

- [ ] **Step 1: Write `docs/dev/os-coverage.md`** (spec: OS coverage honestly scoped)

```markdown
---
title: OS coverage status
date: 2026-08-14
tags: [meta, setup]
summary: Which operating systems the init flow has actually been verified on.
---

# OS Coverage

The template assumes nothing about the target OS, but verification so far is:

| OS | Status |
|---|---|
| Linux | Init rehearsed end-to-end mechanically (see journal 2026-08-14) |
| Windows | Init path reviewed on paper only — VERIFY ON FIRST REAL DEPLOYMENT |
| macOS | Init path reviewed on paper only — VERIFY ON FIRST REAL DEPLOYMENT |

First deployment on an unverified OS: treat init as suspect, journal every deviation,
and fix the `initialize-tool` skill (bump its version) so the next machine benefits.
```

- [ ] **Step 2: Write the journal entry `docs/journal/2026-08-14-template-built.md`**
following the `journaling` skill's format exactly: outcome `done`, tags `[meta, setup]`,
sections filled with what Tasks 1–12 actually did — including, in the dead-ends section,
whatever Task 12 found (or "Nothing failed" plus the rehearsal's evidence lines). This
entry doubles as the format example future sessions imitate, so it must be exemplary:
real commands, real output, real reasons.

- [ ] **Step 3: Index both files**

In `docs/journal/INDEX.md`, replace `_(none yet)_` under `## Entries` with:

```markdown
- 2026-08-14 — [Template built](2026-08-14-template-built.md) — done — meta, setup — how this starter kit was constructed and what the init rehearsal verified.
```

In `docs/INDEX.md`, append under dev docs:

```markdown
- 2026-08-14 — [OS coverage status](dev/os-coverage.md) — meta, setup — which OSes the init flow is actually verified on.
```

- [ ] **Step 4: Verify the whole template one last time**

```bash
grep -c '^<!-- UNINITIALIZED -->$' docs/journal/INDEX.md
ls .claude/skills/_user-level | sort | tr '\n' ' '
git status --porcelain | wc -l   # after the commit below: 0
```

Expected: `1` (marker intact — this repo ships uninitialized); `creating-skills documenting explaining-complexity finding-knowledge git-discipline journaling`; and after Step 5, `0`.

- [ ] **Step 5: Commit**

```bash
git add -A && git commit -m "Journal the template build; record OS coverage honestly

Journal entry doubles as the format example future sessions imitate.
Marker stays: this repo ships as the pristine, uninitialized template."
```

---

## Self-review (done at planning time)

- **Spec coverage:** layout → T1; CLAUDE.md → T2; permissions → T3; six generic skills →
  T4–T9; init flow + machine-wide block + version rule → T10; checklist audit → T11;
  dry-run rehearsal + idempotency + fake home → T12; OS-coverage honesty + journaling the
  work → T13. Out-of-scope items (hooks, graft, network, skill registry) have no tasks, as
  specified.
- **Placeholder scan:** every file's full content is in its task; the only `<…>` markers
  are the ones the template deliberately ships (`<set at init>`), which T10 step 3 defines
  how to replace.
- **Type consistency:** marker string, delimiter strings, skill names, `version` integers,
  and index-line formats are identical across T1, T2, T10, T12, T13 (checked verbatim).
