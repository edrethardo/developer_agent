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
