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
