---
title: Developer agent template built and rehearsed end-to-end
date: 2026-08-14
tags: [meta, setup]
summary: All 13 plan tasks for the developer-agent template are complete, reviewed twice each, and the init flow was dry-run rehearsed on a scratch copy before shipping.
outcome: done
---

# Developer agent template built and rehearsed end-to-end

## What was asked

Build the developer-agent template per the approved spec
(`docs/superpowers/specs/2026-08-14-developer-agent-template-design.md`) and plan
(`docs/superpowers/plans/2026-08-14-developer-agent-template.md`): scaffolding, a lean
`CLAUDE.md`, a curated permission allowlist, six generic skills staged under
`_user-level`, an `initialize-tool` skill, a rule-to-mechanism audit, and a dry-run
rehearsal of the init flow.

## What I did

All 13 plan tasks, executed by subagents with two-stage review per task/batch (spec
compliance first, then quality) — every produced file verbatim-verified against the
plan, then improved through the review findings. In commit order:

- `c6eed74` Add developer agent template design spec
- `3ea5359` Spec self-review: pin skill version format to plain integer
- `36a328d` Add implementation plan for the developer agent template
- `58239b3` Add template scaffolding: README, CHANGELOG, doc indexes, tag vocabulary
- `03a8048` Add project CLAUDE.md: identity, hard rules, stack policy, init trigger
- `a3b5267` Add curated permission allowlist
- `88d8725` Tighten permissions and polish user-facing files per quality review
- `46bfa13` Extend settings ask-guard to settings*.json
- `ff6cc99` Add journaling skill: evidence-first work log with mandatory index line
- `cc5c1fa` Add finding-knowledge skill: indexes before grep, cite instead of re-derive
- `e830846` Add git-discipline skill: branches as undo, clean tree at session end
- `b890f1f` Add explaining-complexity skill: honest cost assessment before building
- `d75247e` Add documenting skill: user docs, dev docs, changelog and indexes in the change's commit
- `f07aefc` Add creating-skills skill: the flywheel that turns recurring work into skills
- `0d583c4` Fix skill inconsistencies found in quality review
- `736ab57` Add initialize-tool skill: idempotent conversational first-run setup
- `7515e4a` Harden initialize-tool against real first-run failures
- `a08bcb0` Fix CHANGELOG done-check that false-positived for English-language users
- `86b2198` Add rule-to-mechanism audit: all nine hard rules have mechanisms
- `122a924` Mark rule 2's mechanism as partial, matching row 9's honesty standard
- `8cdd174` Fix ten ambiguities the init dry-run rehearsal surfaced
- `622df86` Journaling: replace the journal index placeholder on first entry

The dry-run rehearsal itself ran `initialize-tool` against a scratch copy of the repo
under a fake `$HOME`, with German interview answers and tool name "Berichtsmacher", to
confirm the flow works for a real user before anyone else runs it for real.

## What I tried that didn't work — and why

1. **The plan's own verification expectation for `initialize-tool` was wrong.** It
   expected `grep -c 'developer-agent:start v1'` on the skill file to return `2`; the
   actual count is `1` — the file's prose reference to the trigger line omits the "v1"
   suffix, so only the literal trigger line itself matches. The skill file was correct;
   the plan's expected grep count was a miscount. Kept the file as written, noted the
   plan bug rather than "fixing" working code to match a wrong expectation.
2. **First attempt at the six skill commits landed all six files in one commit.**
   Running `git add .claude/skills` on a wholly untracked tree staged every skill file
   at once instead of one per commit as the plan required. Undone with
   `git reset --soft HEAD~1` followed by `git reset` (unstage), then redone one file per
   commit.
3. **Quality review of the permission allowlist caught an overbroad auto-approve.**
   `Edit/Write(~/.claude/**)` would have silently let the tool rewrite the *user's global*
   Claude settings, not just the project's. Narrowed to `~/.claude/skills/**` plus
   `~/.claude/CLAUDE.md` in `88d8725`, and the settings ask-guard was separately widened
   to cover `settings*.json` (not just `settings.json`) in `46bfa13`.
4. **Adversarial review of `initialize-tool` found a hard dead-end on pristine
   machines.** A machine with no git identity configured fails init's first commit with
   git's "Please tell me who you are" error, which the flow had no path around. Fixed in
   `7515e4a` ("Harden initialize-tool against real first-run failures", 12 findings
   total) by adding a repo-local identity step plus two narrowly-scoped `git config`
   allow rules.
5. **The dry-run rehearsal surfaced 12 more findings after all reviews had already
   passed.** The rehearsal (scratch copy, fake `$HOME`, German answers, tool name
   "Berichtsmacher") passed end-state correctness, idempotency (commit count 3→3 on a
   second run, fake-home file checksums byte-identical), and interrupted-and-resumed
   runs — but still found real bugs no review had caught: the identity done-check only
   looked at `user.name` and ignored `user.email`, so a name-only git config would pass
   the check and then fail the actual commit; a crash mid-init could leave a tree that
   gets mislabeled "pristine" on the next run instead of "partially initialized"; and the
   init flow's own journal entry had no canonical filename, so nothing could detect
   "init already journaled" on resume. All fixed in `8cdd174` ("Fix ten ambiguities the
   init dry-run rehearsal surfaced") and `622df86` ("Journaling: replace the journal
   index placeholder on first entry"). This is the concrete argument for rehearsing
   before shipping: two full review passes still missed bugs that only showed up when
   the flow was actually run.
6. **Confirmed the rehearsal never touched the real environment.** After the rehearsal,
   `find ~/.claude -newer <rehearsal-start-marker>` restricted to the sanctioned paths
   returned empty — nothing outside the scratch copy and fake `$HOME` was modified.

## Decisions made

- **Skills ship at `version: 1`.** The version field only earns its keep once deployed
  copies of a skill can diverge from the template's; at release there is exactly one
  copy, so bumping now would be premature churn.
- **Rule 2 in the rule-to-mechanism audit is marked "Partial", not "Done".** `122a924`
  corrected an earlier overclaim so the audit's honesty standard matches row 9's — a
  mechanism that only partially covers a rule should say so, not round up.
- **graft, hooks, and network-dependent features are excluded from this template**, per
  the spec — they're environment-specific and would make the template lie about what it
  actually does out of the box.
- **The template ships with its full build history in git, unsquashed.** Future
  maintainers get the real sequence of mistakes and fixes (this entry included) instead
  of a sanitized single commit — transparency over tidiness.

## Follow-ups

- Verify `initialize-tool` on Windows and macOS at the first real deployment on either
  OS — both are currently reviewed on paper only. See `docs/dev/os-coverage.md`.
- Consider enforcement hooks for the hard rules audited in
  `docs/dev/rule-mechanism-map.md`; deferred per spec, not built in this pass.
