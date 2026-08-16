---
name: journaling
description: Use at session end, after completing or abandoning any task, or when an approach fails — record what happened in docs/journal/ so no future session re-investigates it.
version: 4
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

Add one line to `docs/journal/INDEX.md` under `## Entries`, newest first (replacing the
`_(none yet)_` placeholder if it is still there):

    - YYYY-MM-DD — [title](YYYY-MM-DD-slug.md) — outcome — tags — summary

An entry that is not in the index is a bug. Include the index line in the same commit as
the entry.

## When exactly

- At session end (always — but scaled: a ten-minute change earns three honest lines,
  "nothing failed" is a complete answer to the failures section, and one index line is
  the whole obligation. Long entries are for long or surprising work.)
- Immediately after abandoning an approach, while the evidence is still in context.
- After anything surprising: a wrong assumption, a tool quirk, a decision reversal.
