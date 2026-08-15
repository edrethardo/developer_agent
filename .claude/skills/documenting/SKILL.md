---
name: documenting
description: Use after any user-visible change or architectural decision — update user docs, dev docs, CHANGELOG and the indexes in the same session as the change.
version: 3
---

# Documenting

Docs updated "later" are docs never updated. Same session, same commit as the change.

## Where what goes

- `docs/user/` — the user's language. Task-oriented ("how do I run the monthly report"),
  zero jargon. If the user can't follow it, it isn't done.
- `docs/dev/` — English. Architecture, setup, decision records. A decision record states
  what was decided, why, what was rejected, and links the journal entry of the session
  that decided it (relative markdown link).
- `CHANGELOG.md` — only if this project keeps one (an adopted project may deliberately
  not; its CLAUDE.md says so). The user's language, https://keepachangelog.com style, newest first.
  What changed FOR THE USER. Internal refactors, test-only and doc-only changes are
  omitted.

## Frontmatter — every file in docs/user/ and docs/dev/

    ---
    title: <one line>
    date: YYYY-MM-DD
    tags: [tag]            # from docs/TAGS.md only; new tag = add it there, same commit
    summary: <one sentence>
    ---

One schema everywhere means one grep pattern searches everything.

(Journal entries under `docs/journal/` follow the `journaling` skill instead: same schema plus `outcome`, indexed in `docs/journal/INDEX.md`.)

## Indexes — not optional

Every new or renamed doc under `docs/user/` or `docs/dev/` gets its line in `docs/INDEX.md` (same commit), format: `- YYYY-MM-DD — [title](path) — tags — summary`. An unindexed
doc is a bug.

## When docs outgrow themselves

When CLAUDE.md or a doc grows past comfortable reading, extract the detail into project
skills — `<tool>-reference` (what things are: formats, locations, constants) and
`<tool>-how-to` (how recurring tasks are done) — each marked "extracted from X — keep in
sync". See `creating-skills`.
