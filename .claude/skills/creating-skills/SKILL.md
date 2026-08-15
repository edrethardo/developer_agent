---
name: creating-skills
description: Use when a task type has occurred twice, when the user says "always do it this way", or when following a skill produced a bad result — extract or improve a skill.
version: 3
---

# Creating Skills

Recurring work becomes a skill; failing skills get improved. This is how the system
learns — without it, every session starts from zero.

## When

- The same *shape* of task has happened twice → skill, named after the task
  (`import-monthly-csv`, `generate-quarterly-report`).
- The user states a standing preference ("always format dates like this") → skill, or a
  line added to the existing skill that owns that territory.
- Following a skill produced a bad result → improve THAT skill now: fix the instruction,
  bump `version`, journal what went wrong and what changed (`journaling` skill).

## Writing one

- A folder with a `SKILL.md`; frontmatter: `name`, `description`, `version` (plain
  integer, bump on every change).
- The description is a TRIGGER, not a topic: "Use when <concrete situation>". A skill
  that never fires is dead weight — write the description for the moment of need.
- Short, imperative steps. Reference material goes in extra files in the skill folder,
  loaded only when needed.

## Where a skill lives

Every skill of this tool lives in this repo's `.claude/skills/`. Nothing is installed
machine-wide: skills here load in this project and nowhere else, so improving one can
never change how Claude behaves in the user's other projects. Edit the skill in place
and bump its `version:`.

If the improvement is generic — it would help ANY project using this setup, not just
this tool — also offer to carry it back to the kit: `.developer-agent.json` records
`kit_origin`. If that is a local folder, apply the same change and version bump there
and journal it in both places. If it is only a URL, or the user does not want to bother,
journal the improvement here with a "not carried back" note and move on; the tool keeps
working either way.

## Proven patterns

- One `create-X` skill per recurring content type — a family of small, named skills
  beats one generic "do stuff" skill.
- Mature tools: extract `<tool>-reference` / `<tool>-how-to` project skills from
  overgrown docs (see `documenting`), marked "extracted from X — keep in sync".
