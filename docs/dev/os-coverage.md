---
title: Operating system support
date: 2026-08-14
tags: [meta, setup]
summary: Which operating systems the first-run setup has actually been verified on, and what to do if yours is not one of them.
---

# Operating System Support

This kit assumes nothing about your operating system, but be honest about what has
actually been tried:

| OS | Status |
|---|---|
| Linux | First-run setup verified end-to-end |
| Windows | Reviewed carefully, never run for real — YOU MAY BE THE FIRST |
| macOS | Reviewed carefully, never run for real — YOU MAY BE THE FIRST |

If you are on Windows or macOS and something in the setup goes wrong, that is worth
knowing about, not worth hiding: the assistant should write down exactly what happened
in `docs/journal/`, fix the `initialize-tool` skill in `.claude/skills/`, bump that
skill's version, and — if you got this kit from a shared template — pass the fix back so
the next person does not hit it.

Two known Windows specifics: git is usually not installed (a normal installer, a few
clicks), and after installing it, VSCode must be fully closed and reopened before it
becomes visible.
