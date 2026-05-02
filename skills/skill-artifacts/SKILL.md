---
name: skill-artifacts
description: >
  Load when writing or reviewing skills. Design guidance for SKILL.md
  files and bundled resources.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Skill Artifacts

Skills are reference material loaded into agents. Each skill is a directory
under `skills/` with a `SKILL.md` and optional `resources/`, `scripts/`,
and `assets/` subdirectories.

Descriptions lead with when to load the skill, not what it contains. Most
skills should set `disable-model-invocation: true` and
`allow_implicit_invocation: false` — only exempt skills that serve as safety
nets an agent might need to discover mid-task.

Keep the body under 500 lines. Put deep or variant-specific content in
`resources/` and route to it from the body.
