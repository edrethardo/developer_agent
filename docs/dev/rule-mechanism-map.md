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
| 2 | No low-quality shortcuts; name the temptation | Partial — `explaining-complexity` bans silently building the complex version or quietly delivering less, which covers shortcuts at scoping time; mid-task temptations rely on rule text + rule 9's verification demand |
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
