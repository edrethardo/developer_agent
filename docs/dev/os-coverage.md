---
title: OS coverage status
date: 2026-08-14
tags: [meta, setup]
summary: Which operating systems the init flow has actually been verified on.
---

# OS Coverage

The template assumes nothing about the target OS, but verification so far is:

| OS | Status |
|---|---|
| Linux | Init rehearsed by hand, then run end-to-end by real sessions in an isolated sandbox |
| Windows | Init path reviewed on paper only — VERIFY ON FIRST REAL DEPLOYMENT |
| macOS | Init path reviewed on paper only — VERIFY ON FIRST REAL DEPLOYMENT |

First deployment on an unverified OS: treat init as suspect, journal every deviation,
and fix the `initialize-tool` skill (bump its version) so the next machine benefits.
