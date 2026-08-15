---
title: Re-verification round — eight of ten closed, seven new findings fixed
date: 2026-08-15
tags: [meta, bugfix, decision]
summary: The hostile reviewer confirmed 8/10 fixes and both design deviations, then caught the release contradicting itself and lacking a deployment path to already-deployed tools; all new findings addressed.
outcome: done
---

# Re-verification round — eight of ten closed, seven new findings fixed

## What was asked

Have the original adversarial reviewer re-attack the fix release: verify each fix closes
its concrete failure scenario, judge two deliberate deviations, hunt for new failure
modes the fixes introduced.

## What I did

Verdicts: 8/10 findings closed at template level; both deviations endorsed (ask-not-
remove for `~/.claude` writes; keeping `_user-level/` staging — the reviewer conceded
folder-copy distribution requires it). New findings and their fixes, same day:

- **N2 (worst):** CLAUDE.md still said "edit both copies together" — contradicting the
  redesign inside the very release that fixed copy drift. Fixed: "read-only delivery
  copy … improvements go template-first".
- **N3:** shadow skills had no retirement step and the precedence claim was stated as
  fact. Fixed: retirement rule added (delete shadow when shared version ≥ yours);
  precedence claim now carries its fallback.
- **N4:** the prune step's date heuristic could delete a REAL tool's journal in a
  re-init collision. Fixed: prune is now identity-based — only files byte-identical to
  the repo's first commit are template memoirs; anything differing stays.
- **N5:** the priority order silently made git safety overrule explicit user
  instructions. Made conscious and bounded in the rule text: "don't commit" → park on a
  `wip-` branch, say so, never fight further. Flagged to the owner as a policy choice.
- **N6:** the permission-box explainer ran AFTER the prompt storm it explains. Fixed:
  explainer is now init step 4, before the installs.
- **N7:** "Template:" vs "Template origin:" string mismatch. Fixed.
- **N1 (not fixable from here):** project `settings.json` and CLAUDE.md have no
  propagation channel, so the deployed Werkbank still auto-approves `~/.claude` writes
  and instructs the old both-copies edit. Its repo is owned by another session; that
  session was asked via cross-session message to apply the finite patch (their user
  approves their prompts). Accepted residuals: F2's modified-content edge (foreign
  CONTENT in shipped files still passes the pristine check — a manifest would be
  needed), and integer-version collisions now confined to the template's staging.

## What I tried that didn't work — and why

The first insertion of the shadow-retirement step landed after the list's closing
sentence (list read 1, 2, closing, 3) — caught on re-read, reordered. Nothing else
failed.

## Decisions made

- N5 policy (bounded disobedience on git safety) chosen by the maintainer session and
  flagged to the template owner rather than silently shipped — it is a values choice.
- N1 routed through the owning session instead of editing a foreign repo — ownership
  beats convenience.

## Follow-ups

- N1 CLOSED same day: the Werkbank session verified the claims independently (grep of
  its settings.json, version compare, v1→v2 diff), asked its user, and applied all of
  it — `~/.claude` writes moved allow→ask, CLAUDE.md flywheel wording replaced, the six
  shared skills upgraded to v2, and later `initialize-tool` v3 pulled as well. Its
  install run is also this project's only interactive evidence that the ask-guards fire
  and the install completes after approval (see 2026-08-15-sandbox-init-test.md, where
  headless testing could not reach those steps). Its own `_user-level/` staging stays at
  v1 deliberately — Werkbank is a tool, not a template, and delivers to nobody.
- First real Windows deployment still owes the os-coverage verification.
