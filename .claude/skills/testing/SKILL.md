---
name: testing
description: Use before writing any logic the user will trust — sums, filters, imports, anything money- or record-shaped — and before claiming that anything works.
version: 3
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

Use real examples from the user's own data as cases whenever they give you one — ask
for a small sample file early; it is worth more than invented cases.

## Where tests live

`tests/`, mirroring `src/`. One obvious command runs everything (record it in
`docs/dev/stack.md`). The user must be able to say "run the checks" in plain language
and get a clear answer: what passed, what failed, and what that means for them — never
raw runner output, never a terminal instruction.

## Before saying it works

Run the suite and the tool itself; report what you actually observed. "Tests pass"
without having run them is the exact dishonesty the honest-reporting hard rule forbids.
If something cannot be tested automatically, say so, say how you checked it by hand,
and note it in the journal.
