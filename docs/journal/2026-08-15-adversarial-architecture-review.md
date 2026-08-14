---
title: First adversarial architecture review of the shipped template
date: 2026-08-15
tags: [meta, investigation, decision]
summary: Rule-11 review found ten findings, three structural — the skill-sharing flywheel is architecturally disconnected, shared-skill writes race, and the six skills fire in every repo on the machine.
outcome: done
---

# First adversarial architecture review of the shipped template

## What was asked

Run CLAUDE.md hard rule 11 against the template itself (HEAD after adding rules 10–11),
with a hostile reviewer mandated to attack boundaries, coupling, lifecycle, multi-tool
reality, and the non-technical-user contract.

## What I did

One adversarial subagent read the full tree plus the live deployment evidence on this
machine (`~/.claude/skills/`, the deployed Werkbank tool). Ten findings, ranked. The
first three are structural:

1. `Edit/Write(~/.claude/skills/**)` is auto-approved for every session forever; the
   shared skills are multi-writer with bare-integer versions and no git — a silent
   last-writer-wins race that violates the template's own "auto-approve what git can
   recover" principle.
2. Init's pristine-baseline done-check verifies commit count, not tree content — it
   already mislabeled real WIP once (Werkbank collision, 2026-08-14).
3. The improvement flywheel is disconnected: README says copy the ORIGINAL template,
   so per-tool `_user-level/` staging never propagates; `initialize-tool` fixes in
   deployed tools have no path home; project CLAUDE.md has no propagation at all
   (verified: the deployed tool lacks rules 10–11).

Further: the six generic skills trigger unconditionally in EVERY repo on the machine
(only the CLAUDE.md block was scoped, not the far more powerful skill descriptions);
rule 10's frugality collides with rules 3/4/5/6 ceremony on small tasks with no stated
priority; the hard rules exist in three copies with the spec's numbering already stale;
the template's build journal ships into every tool's mandated memory; skills hardcode
settings.json behavior across a layer boundary; the machine-wide block's skill list and
paths go stale by design (accepted, cosmetic).

## What I tried that didn't work — and why

The review itself caught a live rule violation by this session: commits `9befc67`,
`8403862`, `1fd9ae6` (rules 10 and 11) each changed CLAUDE.md and the audit map with no
journal entry — evidence that rule 5 does not self-enforce even on the template's own
authors. This entry is the correction.

## Decisions made

- Findings journaled first, fixes second — the structural rework (single-owner skill
  home, read-only `~/.claude/skills/`, recorded template origin, dropping staging
  copies) changes the deployment model and belongs to the template owner, not to a
  review session. Ticketed below as follow-ups pending that decision.
- Finding 10 (block staleness) accepted as designed; the block's own stale-path warning
  is the mitigation.

## Follow-ups

- Owner decision needed: adopt the single-owner flywheel redesign (findings 1, 3, 8) —
  this is a v2 deployment-model change.
- Cheap fixes ready to apply: content-aware pristine check (2), scoping clauses in the
  six skill descriptions (5), a proportionality/priority clause for the rules (6),
  README Windows/OneDrive placement guidance (4), conditional wording for settings
  claims in skills (9), spec note that the rule set grew 7→11 (7).
