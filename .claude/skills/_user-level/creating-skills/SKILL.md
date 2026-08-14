---
name: creating-skills
description: In developer-agent tool repos (the repo carries docs/journal/INDEX.md): use when a task type has occurred twice, when the user says "always do it this way", or when following a skill produced a bad result — extract or improve a skill.
version: 2
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

## Which layer — and where improvements go

- **Tool-specific** (mentions this tool's files, data, or quirks) → this repo's
  `.claude/skills/`.
- **Generic** (would help ANY tool repo) → the improvement's home is the TEMPLATE, not
  this machine's shared copies. NEVER edit `~/.claude/skills/` directly: it is a
  read-only deployment target that only the template's init step writes, and a direct
  edit starts a silent race with every other tool on this machine.
  1. Shadow locally first: copy the skill folder into this repo's
     `.claude/skills/<name>/`, apply the change, bump `version`. Project skills take
     precedence over same-named user-level ones, so this tool benefits immediately (if
     the harness ever resolves otherwise, rename the project copy and use it by name).
  2. Propagate to the template: its origin is on the "Template origin:" line of the
     machine-wide `~/.claude/CLAUDE.md` block. If that is a local folder, apply the
     same change (same version bump) to the template's
     `.claude/skills/_user-level/<name>/` and journal it there; if it is only a URL or
     unreachable, journal the improvement HERE with a "not yet propagated" follow-up so
     it can be carried over later.
  3. Retire the shadow: whenever the shared copy in `~/.claude/skills/<name>/` reaches a
     `version` ≥ your shadow's, delete the shadow and journal it — a lingering shadow
     pins this tool to an aging fork forever.
  Future tools receive the improvement at their init through the version compare.

## Proven patterns

- One `create-X` skill per recurring content type — a family of small, named skills
  beats one generic "do stuff" skill.
- Mature tools: extract `<tool>-reference` / `<tool>-how-to` project skills from
  overgrown docs (see `documenting`), marked "extracted from X — keep in sync".
