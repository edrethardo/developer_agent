---
name: git-discipline
description: Use before starting work, after reaching any working state, at session end, and when the user asks to undo or go back — keeps every change recoverable and the history readable.
version: 3
---

# Git Discipline

The repo is local-only. Branches are an undo mechanism here, not a review mechanism.
History is the backup — treat it that way.

## Rules

- `main` is the working branch; commit directly for routine work.
- Commit at every working state — "working" means the checks pass (`testing` skill),
  not merely that the file was saved. Small commits beat big ones.
- Message format: first line = what changed for the tool; body = why, when the why isn't
  obvious. English. Written so a future session — or a hired developer — can follow the
  history without asking anyone.
- Risky or experimental work: branch first (`git checkout -b try-<thing>`), merge when it
  works. Abandoned experiment: journal it (what was tried, why abandoned — `journaling`
  skill), then delete the branch. (`git branch -D` — under the template's stock allowlist
  this shows a permission box by design; tell the user in one plain sentence what is
  being deleted and why before confirming.)
- Docs, CHANGELOG and index updates go in the SAME commit as the change they describe.
- Session end = clean tree (`git status` shows nothing) and a journal entry. Genuinely
  half-done work: commit it on a branch named `wip-<thing>` with a commit message starting `wip:` plus a journal entry saying
  exactly where it stands.
- Never rewrite history: no `reset --hard`, no `--amend` (not even on the latest commit), no force anything.
  A wrong commit is fixed by a new commit.

## Explaining git to the user

Only when they need to care. "I save a snapshot after every change, so we can always go
back" covers most conversations. Never make the user run git commands.
