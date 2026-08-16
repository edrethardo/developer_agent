---
name: reviewing
description: Use when writing acceptance criteria, when about to report something as verified or done, and when the hard rules call for an adversarial review — every check has to say what it cannot see.
version: 3
---

# Reviewing

A check that passes tells you what it looked at, not what is true. This skill is
about the gap between those two, because that gap is where the damage lives.

## Every check names its blind spot

Whenever you define acceptance criteria, or report that something is verified,
add one sentence: what this check does NOT cover. Not a disclaimer — a specific,
useful sentence a reader could act on.

> "Tests pass" covers the logic; it does not cover whether documents stayed
> intact, whether anything was written outside the repo, or whether the guard is
> actually called.

If you cannot name a blind spot, you have not understood the check yet.

## The blind spots that keep recurring

Measured, each one from a real failure:

- **Deletions.** A test suite cannot see a document that was shortened or
  emptied. A changelog once went from 428 lines to 30 behind a green gate.
- **Other media.** A text search cannot see an image, a PDF, a binary. Seven
  redaction checks passed over a photo that then shipped publicly.
- **Outside the repo.** `git status` cannot see the home directory, config and
  state folders, other projects. A test suite deleted real log files there for
  days without a single visible symptom.
- **The wiring.** A unit test proves the guard works, not that anyone calls it.
  Deleting an authentication check left 280 tests green.
- **The run's own side effects.** Checking whether something works can itself
  change or destroy state. Ask what running the check does.

## Fresh eyes find what you cannot

A session cannot find the assumption it made itself. Have the adversarial review
done by someone with no history of the work: a separate session with its own
context, read-only, reporting findings back — and a different model where one is
available, because a different model brings different blind spots rather than the
same ones twice.

Measured: self-reviews of one project caught real defects, but two reviewers
without prior context found the two worst ones — a destroyed changelog and a
privacy leak — independently of each other, within an hour, in a codebase the
original sessions had reviewed repeatedly.

In this setup that realistically means one of three, in descending strength: a
different model if the user has one; otherwise a fresh subagent with no history of the
work, which at least has not made your assumptions even if it shares your habits; and
if neither is possible (cost, time, a single seat), do the review yourself and say
plainly in the result that it was a self-review and therefore weaker. Never present a
self-review, or a subagent's, as an independent one.

## Read what you delegated

Work handed to another session, another model, another lane or a subagent is
reviewed by whoever handed it over, before it is accepted:

- Read the whole diff, not the summary. Their acceptance criterion is never the
  only evidence — measured: a delegated run deleted 413 lines of a user's
  changelog and reported success, because its gate was "tests pass".
- Look hardest at files the task never mentioned. That is where a delegate's
  side effects land.
- A report of success with no diff to inspect is not a result. Ask for it, or
  reproduce the work yourself.

## Doing an adversarial review

When the hard rules call for one, or the user asks:

1. Say what it costs before starting, and let the user decide.
2. Attack, do not survey: hunt for what breaks, in which concrete scenario, for
   whom. A finding without a scenario is an opinion.
3. Every finding carries file:line, a concrete failure ("user does X, Y happens
   silently"), and its rank by likelihood x damage. Drop cosmetic findings
   entirely rather than padding the list.
4. Verify before claiming. Read the code, run the thing, quote real output.
   A plausible-sounding finding that turns out to be wrong costs you the reader's
   trust for the real ones.
5. Change nothing during the review. Findings go to the user, and to the journal
   (`journaling` skill); fixes are a separate, agreed step.
6. End with the honest verdict, including "nothing worth fixing" when that is
   what you found.
