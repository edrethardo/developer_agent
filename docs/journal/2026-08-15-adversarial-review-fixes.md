---
title: Fixes for all ten adversarial-review findings
date: 2026-08-15
tags: [meta, decision, bugfix]
summary: The flywheel is now one-directional and template-first, ~/.claude is a prompted deployment target, skill triggers are scoped to tool repos, init checks tree content before claiming pristine and prunes template history.
outcome: done
---

# Fixes for all ten adversarial-review findings

## What was asked

Fix all ten findings of the 2026-08-15 adversarial review (see that entry).

## What I did

- **F1 (write race):** `Edit/Write(~/.claude/**)` moved from allow to ask — `~/.claude`
  is not git-recoverable, so by the template's own principle it must prompt. Init now
  pre-announces the install prompts in plain language.
- **F3/F1 (flywheel):** `creating-skills` v2 — generic improvements are made as a
  project-skill shadow locally, then propagated to the TEMPLATE (origin recorded on the
  machine-wide block's "Template origin" line, block bumped to v2); editing
  `~/.claude/skills/` directly is declared a bug. `_user-level/` staging stays as the
  offline delivery vehicle only.
- **F2 (false pristine):** init's baseline step now inspects tree content and stops to
  ask when non-template files are present — the exact Werkbank-collision bug.
- **F5 (unscoped triggers):** all six generic skills' descriptions now begin "In
  developer-agent tool repos (the repo carries docs/journal/INDEX.md)"; all bumped to
  version 2 so deployed machines upgrade on next init.
- **F6 (rule collisions):** CLAUDE.md hard rules gained a proportionality clause and an
  explicit priority order (honesty > git safety > user instruction > frugality > rest).
- **F4 (Windows/OneDrive):** README warns against sync-folder placement and honestly
  names the one-installer git exception; init handles the no-shell Windows case without
  looping on errors.
- **F8 (shipped memoirs):** init v2 prunes the template's own journal entries and index
  lines from a new tool (they remain in the template repo and the baseline commit).
- **F7 (copy drift):** spec's post-implementation notes now record the 7→11 rule growth
  and the flywheel redesign; the last by-number rule reference in initialize-tool is
  content-based; audit row 7 updated.
- **F9 (layer claims):** git-discipline's and initialize-tool's statements about
  permission prompts are now conditional on the stock allowlist.
- **F10 (block staleness):** accepted as designed (block warns and readers must verify);
  no change.
- Shipped a `.gitignore` (OS droppings, `settings.local.json`, common caches).

## What I tried that didn't work — and why

Nothing failed. One rejected alternative worth recording: keeping `~/.claude` writes
auto-approved for init UX. Rejected because the reviewer showed the allow list's only
non-git-recoverable target was exactly the shared state every tool depends on; a few
narrated one-time prompts are the honest price.

## Decisions made

- The template origin line defaults to the public GitHub URL — init cannot detect where
  a folder copy came from, and the URL is the one origin that is always true; machines
  keeping a local template clone edit the line once.
- All six skills and initialize-tool jumped to version 2 in one release so the deployed
  machine (this one) upgrades atomically at its next init.

## Follow-ups

- The deployed Werkbank tool still runs v1 skills and a v1 machine-wide block; they
  upgrade at the next template init on this machine (or its owner session can pull the
  v2 template deliberately).
- Re-verification by the original adversarial reviewer pending (this session).
