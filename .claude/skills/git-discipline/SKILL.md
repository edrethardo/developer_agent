---
name: git-discipline
description: Use before starting work, after reaching any working state, at session end, and when the user asks to undo or go back — keeps every change recoverable and the history readable.
version: 6
---

# Git Discipline

The repo is local-only. Branches are an undo mechanism here, not a review mechanism.
History is the undo — but it sits on the same disk as everything else, so it is NOT a
backup (see below).

## Rules

- `main` is the working branch; commit directly for routine work.
- Commit at every working state — "working" means the checks pass (`testing` skill),
  not merely that the file was saved. Small commits beat big ones. One deliberate
  exception: a failing test committed on its own as proof of test-first (`testing`
  skill) — its message says so, and the fix follows in the next commit.
- **Before every commit, read your own diff.** `git status --short` and
  `git diff --stat` (or `--cached`), then three questions: is a file in there I
  did not mean to touch? does it delete something I did not mean to delete? does
  the size match what I actually did? Any unexpected deletion or unexpected file
  STOPS the commit — look at what happened and tell the user what you found.
  Measured why: a run once cut a user's CHANGELOG from 428 lines to 30 and
  shipped it green, because its check was "tests pass" and no test can see a
  deleted document; the next commit touched that same file and still missed it.
- **A shared file you did not create — a changelog, a manual, an index — gets a
  glance before you commit it**: is it still as long as it was, is its beginning
  still there? You are editing something the user reads.
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

## Backups — git is not one

Everything you protect with git lives on one computer. A dead disk, a stolen laptop or a
wrong folder deletion takes the tool, its whole history and the user's data files at
once. Their data is not even in git (see the data rule in CLAUDE.md), so for those files
there is no undo at all.

- **Ask once, early** — as soon as the tool does something worth keeping: is there a
  copy of this folder anywhere else? Record the answer in `.developer-agent.json` as
  `"backup"`: where it is, or `"none"`, or `"declined"`.
- **If there is none, nag — but gently and rarely.** At most once per session, and only
  after something worth losing was added, say plainly: THIS TOOL AND YOUR FILES EXIST
  ONLY ON THIS COMPUTER. Then offer to do it rather than assigning homework: "tell me a
  second location — a USB stick, a network drive — and I will copy the whole folder
  there now." Copying outside the project shows a permission box; explain it in one
  sentence.
- **Do it properly when they accept**: copy the entire folder, data files included, and
  say what was copied and where. Update `"backup"` and journal it.
- **Respect a no.** After they decline twice, record `"declined"` and stop asking —
  raise it again only before something genuinely risky, or if they ever say they lost
  something.
- A sync folder (OneDrive, Dropbox) is not a good home for the tool itself, but it is a
  perfectly good place to keep a COPY. Say that if they have one.

## Explaining git to the user

Only when they need to care. "I save a snapshot after every change, so we can always go
back" covers most conversations. Never make the user run git commands.
