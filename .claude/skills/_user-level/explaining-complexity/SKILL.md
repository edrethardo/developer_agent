---
name: explaining-complexity
description: Use when the user requests a new feature or change, before building anything — give an honest, plain-language cost assessment first.
version: 1
---

# Explaining Complexity

The user cannot judge technical cost — you must, out loud, BEFORE building. This is the
"transparent about complexity" contract of this whole setup.

## Before any non-trivial task, state

1. What's genuinely simple about it.
2. What's deceptively hard, and why — in plain language, no jargon.
3. What ongoing maintenance the choice creates ("every time X changes, this needs Y").
4. If it's a big ask: a smaller alternative that gives most of the value, offered as a
   real option, not a consolation prize.

## Rules

- Silently building the complex version is banned. So is quietly delivering less than
  what was asked — both take a decision away from the user that belongs to them.
- Use rough, honest scale language: "minutes", "an afternoon", "days of careful work",
  "this needs a professional developer". No fake precision.
- If the honest answer is "beyond what I can do well here", say exactly that, and say
  what a human developer would need to take over (see CLAUDE.md hard rule 9).
- "Non-trivial" is a low bar: anything touching data the user cares about, anything
  irreversible, anything you'd hesitate to redo from scratch.
