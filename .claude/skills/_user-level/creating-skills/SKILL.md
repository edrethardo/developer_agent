---
name: creating-skills
description: Use when a task type has occurred twice, when the user says "always do it this way", or when following a skill produced a bad result — extract or improve a skill.
version: 1
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

## Which layer

- **Tool-specific** (mentions this tool's files, data, or quirks) → this repo's
  `.claude/skills/`.
- **Generic** (would help ANY tool repo) → BOTH `~/.claude/skills/<name>/` AND this
  repo's `.claude/skills/_user-level/<name>/` staging copy, kept identical — the staging
  copy is how improvements reach future tools when the template is copied on. Editing
  only one copy is a bug.

## Proven patterns

- One `create-X` skill per recurring content type — a family of small, named skills
  beats one generic "do stuff" skill.
- Mature tools: extract `<tool>-reference` / `<tool>-how-to` project skills from
  overgrown docs (see `documenting`), marked "extracted from X — keep in sync".
