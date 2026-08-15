# Developer Agent Template — Design

**Date:** 2026-08-14
**Status:** Approved pending user review

## What this is

A template repository for Claude Code (VSCode extension). Non-technical users copy the
folder, open it in VSCode, and talk to Claude to build their own internal tools. The
template is the agent's operating system: it makes the agent behave like a disciplined
professional developer — git-driven, honest about complexity, allergic to low-quality
shortcuts, self-documenting, and steadily extracting recurring work into reusable skills.

The deliverable of *this* repo is the template itself, not any particular tool.

## Decisions made during brainstorming

| Question | Decision |
|---|---|
| Deployment model | **Template**: copied once per internal tool; each tool is its own repo |
| Machine-wide setup | First init on a machine also configures user-level `~/.claude/CLAUDE.md` (append-only delimited block, never overwrites existing content) |
| Skill layering | **Layered**: generic developer-discipline skills install to `~/.claude/skills/` at init; tool-specific skills live in the project's `.claude/skills/` |
| Tool stacks | **Stack-agnostic** with a documented preference order: simplest thing that works, widely-installed runtimes, minimal dependencies, no framework until a real need appears; the choice is documented in `docs/dev/` |
| First-run experience | **Conversation-driven init** — no setup script, no terminal use by the human. CLAUDE.md carries an init trigger; the `initialize-tool` skill walks through setup step by step |
| Distribution | **Folder copy** (zip/USB/share) — no git host, no network assumed. `git init` happens locally at init |
| Target OS | **Unknown** (Windows/macOS/Linux all possible). Nothing may assume bash, Python, or preinstalled git; init checks prerequisites and adapts commands to the detected OS |
| Language | **Chosen at init**, recorded in project CLAUDE.md. User-facing docs and conversation in the user's language; code, commits, dev docs, journal, skills in English |
| Permissions | **Curated allowlist** shipped in `.claude/settings.json`. Principle: auto-approve what git can recover, prompt for what it can't. Allowlist-change requests require a plain-language risk explanation (worst case included) before being added |
| Architecture approach | **Skill-centric** (approach 2 of 3): thin CLAUDE.md + operational behavior in skills; enforcement hooks deliberately deferred (they need a runtime the target machines may not have) |

## Template repository layout

```
developer_agent/                  <- user renames to the tool's name at init
├── CLAUDE.md                     <- agent operating rules (lean, ~80 lines max)
├── README.md                     <- for the human: "open this in VSCode, talk to Claude"
├── CHANGELOG.md                  <- user-facing change log (user's language, Keep-a-Changelog style)
├── .claude/
│   ├── settings.json             <- curated permission allowlist
│   └── skills/
│       ├── initialize-tool/      <- project skill: first-run interview + setup
│       └── _user-level/          <- staging: generic skills that init installs to ~/.claude/skills/
│           ├── journaling/
│           ├── documenting/
│           ├── git-discipline/
│           ├── explaining-complexity/
│           ├── creating-skills/
│           └── finding-knowledge/
├── docs/
│   ├── INDEX.md                  <- topic-grouped index over user/ and dev/
│   ├── TAGS.md                   <- controlled tag vocabulary
│   ├── user/                     <- docs the tool's owner reads (their language)
│   ├── dev/                      <- technical docs: architecture, decisions, setup (English)
│   ├── journal/                  <- searchable work log (English)
│   │   └── INDEX.md              <- chronological index; contains UNINITIALIZED marker in template
│   └── superpowers/
│       ├── specs/                <- YYYY-MM-DD-<topic>-design.md
│       └── plans/                <- YYYY-MM-DD-<topic>.md
└── src/                          <- empty; the tool grows here, stack chosen per tool
```

Design notes:

- Generic skills ship **inside the template** (`_user-level/`) because folder-copy
  distribution allows no network fetch. Init copies them to `~/.claude/skills/`. Each
  skill carries a `version` in frontmatter (a plain integer, incremented on every change);
  init only overwrites an older version — never
  downgrades, never touches skills it did not ship. Template improvements therefore
  propagate at the next init on that machine.
- `src/` starts empty. The stack preference order lives in CLAUDE.md, not scaffolding.

## Project CLAUDE.md

Kept under ~80 lines so it survives long sessions. Contents:

- **Identity & audience.** "You are the sole developer of this internal tool. Your user
  is not technical. They own what the tool does; you own how it is built, and you are
  accountable for its quality."
- **Hard rules** (each maps to a skill or structure — see Checklist audit):
  1. Work in git: meaningful commits at every working state; never end a session dirty.
  2. No low-quality shortcuts. When one is tempting, say so and explain the cost of both paths.
  3. Before any non-trivial task, state its real complexity in plain language
     (`explaining-complexity`).
  4. Journal every session (`journaling`); search journal and docs before re-investigating
     anything (`finding-knowledge`).
  5. Recurring task → extract a skill; failed skill → improve it (`creating-skills`).
  6. Allowlist additions require a plain-language risk explanation first.
  7. Report what actually happened. No success claims without verification. When a task
     exceeds what the agent can do well, say so — including "this needs a human developer"
     when true.
- **Stack preference order** and pointer to docs structure.
- **Init trigger:** "If `docs/journal/INDEX.md` contains the `UNINITIALIZED` marker,
  invoke `initialize-tool` before anything else."
- **User's language** (written by init).

## Machine-wide `~/.claude/CLAUDE.md` block

Written by init, once per machine, as a clearly delimited append-only block (like graft's
`<!-- graft:start/end -->` pattern). Never modifies content outside its delimiters. Skips
if present; updates only if the template's block version is newer. Contents:

- The shared skills exist in `~/.claude/skills/` and should be used.
- The search-before-reinvestigating principle.
- A list of tool repos on this machine (name + path), appended by each init, so tools can
  find each other's docs. The block itself warns the list may be stale ("verify paths exist").

## Init flow (`initialize-tool` project skill)

Conversational, ordered, idempotent — every step detects its own completion, so an
interrupted init resumes cleanly next conversation (the `UNINITIALIZED` marker survives
until the end).

1. **Prerequisite check.** Detect OS; check git exists. If missing: explain what git is
   and why it is needed, then guide the user through installing it for their OS. This is
   the one step that may need the human to click an installer — the skill says so honestly.
2. **Interview** — one question at a time, in the language of the user's first message:
   preferred language (recorded), what the tool should do (short paragraph), tool name.
3. **Mechanical setup**, narrated briefly: `git init` + initial commit of the pristine
   template → write language and tool description into CLAUDE.md and `docs/user/about.md`
   → replace the `UNINITIALIZED` marker → suggest the user rename the folder (the agent
   cannot safely rename its own cwd; flagged as the one manual, skippable step).
4. **User-level skill install** per the version rule above.
5. **Machine-wide CLAUDE.md block** as specified above.
6. **Permissions explainer**: one short paragraph on what runs unprompted, what prompts,
   and how to think about a prompt when it appears.
7. **First journal entry** (documenting the init), then hand-off: "Setup's done. Tell me
   what you'd like the tool to do first."

## Skill library

Every skill: a folder with `SKILL.md`; frontmatter `name`, `description` (written as a
concrete "Use when…" trigger, not a topic label), `version`. Short and imperative;
reference material in extra files loaded on demand.

### Generic skills (installed user-level at init)

- **`journaling`** — Use at session end, after completing or abandoning any task, when an
  approach fails. One file per entry: `docs/journal/YYYY-MM-DD-<slug>.md`, frontmatter
  `title, date, tags, summary, outcome (done|partial|failed|abandoned)`. Body sections:
  What was asked / What I did / What I tried that didn't work and why / Decisions /
  Follow-ups. The dead-ends section is mandatory and must carry evidence (commands run,
  numbers observed) — modeled on DungeonEngine's postmortem style. Every entry gets an
  index line in `docs/journal/INDEX.md` (newest first). An unindexed entry is a bug.
- **`finding-knowledge`** — Use before starting any task, investigation, or debugging, and
  before answering "how does X work?". Order: `docs/journal/INDEX.md` → `docs/INDEX.md` →
  follow tags → only then grep the codebase. Rule: never re-investigate what a past
  session already learned.
- **`git-discipline`** — Use before starting work, after each working state, at session
  end. Local-only repo, `main` as working branch. Branches are an undo mechanism, not a
  review mechanism: risky work happens on a branch, merges when working; abandoned
  experiments are journaled then deleted. Commit messages: first line = what changed for
  the tool, body = why, English. Docs/CHANGELOG updates belong in the same commit as the
  change they describe. Session end = clean tree; genuinely half-done work is committed on
  a `wip:` branch with a journal entry stating exactly where it stands.
- **`explaining-complexity`** — Use when the user requests a feature or change. Before
  building: what is genuinely simple, what is deceptively hard, what ongoing maintenance
  the choice creates; when the honest answer is "this is a big ask," offer a smaller
  alternative with most of the value. Building the complex version silently is banned.
- **`documenting`** — Use after any user-visible change or architectural decision, in the
  same session. Maintains: `docs/user/` (user's language, task-oriented, no jargon),
  `docs/dev/` (English; architecture, decision records with reasons, setup),
  `CHANGELOG.md` (user's language, what changed *for the user*; internal refactors
  omitted), and the indexes. Dev docs link to the journal entries behind decisions.
- **`creating-skills`** — the flywheel. Use when a task type has occurred twice, when the
  user says "always do it this way," or when following a skill produced a bad result.
  Covers: writing a new skill (trigger-focused description, minimal steps, versioned);
  choosing the layer — generic skills go user-level AND are copied back into the
  template's `_user-level/` staging so future tools inherit them; tool-specific skills go
  to project `.claude/skills/`; improving skills (bump version, journal the why).
  Encodes two proven DungeonEngine patterns: recurring content tasks become named
  `create-X`-style skills, and mature tools maintain extracted `<tool>-reference` /
  `<tool>-how-to` project skills with an explicit "extracted from X; keep in sync" note.

### Project skills

`initialize-tool` (above), plus whatever the tool accumulates over its life.

### Known weakness, stated honestly

Skills only fire if their descriptions trigger. Mitigations: CLAUDE.md hard rules name the
skills at their trigger moments, and descriptions are concrete "use when" conditions. This
is a real failure mode of all skill systems, not fully solvable without hooks (deferred).

## Searchability system

Mechanical, not aspirational; plain markdown + grep, zero runtime dependencies:

- **Uniform frontmatter** on every file under `docs/`: `title, date, tags, summary`
  (journal entries add `outcome`). One grep pattern searches everything.
- **Two mandatory indexes**: `docs/journal/INDEX.md` (chronological) and `docs/INDEX.md`
  (topic-grouped over `user/` and `dev/`). One line per file: date, title, tags, summary.
  Reading two files yields a complete map of everything ever written.
- **Controlled tags** in `docs/TAGS.md`: each tag with a one-line meaning. Reuse before
  invent; a new tag must be added to `TAGS.md` in the same commit.
- **Index maintenance is a step of writing**, enforced by `journaling` and `documenting`.

## Permissions (`.claude/settings.json`)

Principle: **auto-approve what git can recover, prompt for what it can't.**

- Pre-approved: read-only inspection; git operations that cannot lose work (`status`,
  `diff`, `log`, `add`, `commit`, `branch`, `checkout -b`); file edits inside the repo.
- Still prompts: deletion/overwrite (`rm`, `git reset`/`checkout --`/force flags),
  network access, writes outside the repo (except the sanctioned `~/.claude` path used by
  init), software installation.
- Exact patterns are an implementation detail to tune; the principle above is the spec.
- User asks to allowlist something → agent explains in plain language what that command
  class can do, worst case included, before adding it.

## Error handling posture

CLAUDE.md hard rule 7: report what actually happened; no unverified success claims;
failed approaches go to the journal, not into silent retries. When the agent hits its
ceiling it says so and describes what a human would need to do — including "this needs a
professional developer" when true.

## Testing the template

1. **Dry-run rehearsal**: copy the template to a scratch directory; run a full init
   conversation; verify idempotency by interrupting and resuming init at each step.
2. **Checklist audit**: every CLAUDE.md hard rule must map to a skill or structure that
   operationalizes it. A rule with no mechanism is either given one or cut.
3. **OS coverage, honestly scoped**: Linux verified end-to-end here; Windows/macOS init
   paths reviewed but marked "verify on first real deployment" in `docs/dev/`.

## Explicitly out of scope (v1)

- Enforcement hooks (need a guaranteed runtime; revisit per-OS after real deployments).
- Graft integration (requires a locally installed fork; the markdown index system is the
  dependency-free equivalent of its search-before-grep discipline).
- Any network/git-host features (remotes, PRs, CI).
- Skill distribution beyond "copied at init" (no registry, no auto-update).

## Post-implementation notes (2026-08-14)

Recorded after execution; the implementation deviates from this spec in two reviewed,
deliberate ways (see `docs/journal/2026-08-14-template-built.md` for the full story):

- "Uniform frontmatter on every file under `docs/`" is implemented as: every file under
  `docs/user/`, `docs/dev/` and `docs/journal/`. Files under `docs/superpowers/`
  (specs/plans, this file included) follow the superpowers naming convention instead.
- The permission allowlist and the `initialize-tool` skill were hardened beyond this
  spec's sketch during quality review (narrower `~/.claude` scopes, ask-guards on
  permission-bearing files, git-identity fallback, crash-path labeling, and more) —
  same principles, stricter execution.
- The hard-rule set grew from this spec's 7 to 11 (docs-same-commit split out; added:
  token frugality, adversarial-review cadence). CLAUDE.md is canonical; the audit map
  mirrors it; this spec's rule list is historical.
- 2026-08-15, after the first rule-11 adversarial review: the skill flywheel was made
  one-directional (improvements go template-first via the machine-wide block's
  "Template origin" line; `~/.claude/skills/` is a read-only deployment target and its
  writes now prompt), skill triggers were scoped to tool repos, and init prunes the
  template's own journal entries from new tools. The build/review journal is not shipped in the template tree; it lives in the
  template repository's git history.
