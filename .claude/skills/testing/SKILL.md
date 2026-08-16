---
name: testing
description: Use before writing any logic the user will trust — sums, filters, imports, anything money- or record-shaped — and before claiming that anything works.
version: 6
---

# Testing

The user cannot check your work. A wrong number in their report looks exactly like a
right one, so the tool must prove itself.

## What gets a test — and what doesn't

Test anything that COMPUTES something the user will trust: totals, sums, filters,
date/period logic, imports and exports, anything touching money, invoices or records,
and every bug you fix (a test that fails before the fix, passes after).

Do not test: framework or library behaviour, trivial glue with no logic, cosmetic
output. A test suite full of noise gets ignored, which is worse than none.

## Write the test first

For anything in the "gets a test" list:

1. Write the test, stating the expected answer as a plain fact ("a January export with
   three receipts and one missing gives a total of 412.50 and one flagged row").
2. Run it. It MUST fail, for the reason you expect — a test that passes before the code
   exists is testing nothing.
3. Write the smallest code that makes it pass.
4. Run it again. Green.
5. Tidy the code with the test still green, then commit test and code together.

Ask the user to DESCRIBE a real case early — the columns, a typical row, the awkward
one that breaks things — and build the test data from that description. Never copy their
real file into the tests: fixtures are committed forever. If you must see the format,
read their file where it lives, take the shape, and invent rows that match it.

## Where tests live

`tests/`, mirroring `src/`. One obvious command runs everything (record it in
`docs/dev/stack.md`). The user must be able to say "run the checks" in plain language
and get a clear answer: what passed, what failed, and what that means for them — never
raw runner output, never a terminal instruction.

## Test the seam, not only the unit

A rule that is enforced somewhere is only as good as the call site. Test the
place that INVOKES it, not just the logic it invokes.

- **The deletion test:** pick the line that enforces the rule — the auth check,
  the path bound, the limit — and ask "if I delete this line, does a test go
  red?" If no, the test you need does not exist yet. Measured: deleting an
  authentication check from a request handler left all 280 tests passing,
  because every test called the guard function directly and none went through
  the handler.
- **Bounds come from the caller.** A guard is usually correct and the caller
  usually decides what it guards. Widening the caller's bounds can open
  everything while the guard's own tests stay green — so one test must exercise
  the real path with the real bounds.
- **Tests that read source text or reimplement logic do not count as proof.**
  Asserting that a file contains a string, or mirroring browser logic in another
  language, may support a real test but must never be the only one — a mirror
  drifts silently, and a scraped constant with a fallback default keeps testing
  the old value after a rename.

## Tests touch nothing outside a temp directory

A test run must be repeatable and harmless. If running the suite can change or
destroy something the user cares about, the suite is a liability — and the damage
is usually invisible, because it happens where `git status` cannot see it.

- **List the write paths before the first test.** Everything the code writes:
  home directory, config, state and log directories (`~/.config`, `~/.local/state`,
  `%APPDATA%`), the system temp dir, other projects, anything from an environment
  variable. Every one of them gets redirected in tests to a fresh temp directory.
- **A path that cannot be redirected is a bug in the code, not in the test.** Make
  the location injectable (argument, or an environment variable the code reads)
  and say so in the journal.
- **Redirect where the runner actually looks.** `python -m unittest discover` does
  NOT import `tests/__init__.py`, so isolation placed in a package init silently
  does nothing under the default runner. Put the redirect in the test module
  itself, BEFORE it imports the code under test, and pin that with a test.
- **No real identifiers in fixtures.** Real record ids, real filenames, real ports
  collide with real data. Invent values that cannot exist in production.
- **Check afterwards, outside the repo too.** A clean `git status` proves nothing
  about `~/.local/state`. Compare the paths you listed — modification times or
  checksums — before and after the run, and say what you compared.

## Before saying it works

Run the suite and the tool itself; report what you actually observed. "Tests pass"
without having run them is the exact dishonesty the honest-reporting hard rule forbids.
If something cannot be tested automatically, say so, say how you checked it by hand,
and note it in the journal.
