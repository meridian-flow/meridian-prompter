---
name: skill-artifacts
description: >
  Schema and structure for skill definitions — the SKILL.md files and
  bundled resources that provide shared knowledge to agents. Load when
  writing or reviewing skills.
---

# Skill Artifacts

Skills are reference material loaded into agents. They live in a `skills/` directory.

## Directory Structure

```
skills/
  skill-name/
    SKILL.md              # Required. Main skill file.
    resources/           # Optional. Deep content.
      topic-a.md
      topic-b.md
    scripts/              # Optional. Executable code.
    assets/               # Optional. Templates, icons, etc.
```

## SKILL.md Structure

```markdown
---
name: skill-name          # Required. Matches directory name.
description: >            # Required. When to load this skill.
  What this skill provides and trigger phrases.
---

# Skill Name

Overview of what this skill covers.

## Section 1
Core content.

## Section 2  
More content.

## References
Pointers to deeper content in resources/ directory.
```

## Frontmatter Schema

```yaml
---
name: skill-name                    # Required. Matches directory.
description: >                      # Required. Triggering surface.
  What this skill provides. Include trigger phrases and contexts.
  Be specific about when to load it.
compatibility: [tool-x, tool-y]     # Optional. Required tools/dependencies.
---
```

### Description as Trigger

The description is the primary mechanism for skill triggering. Include:
- What the skill does
- Specific contexts for when to use it
- Trigger phrases users might say

Be slightly "pushy" — undertriggering is more common than overtriggering.

## Progressive Disclosure

Three levels with different token costs:

| Level | Size | When Loaded |
|-------|------|-------------|
| Metadata | ~100 words | Always in context |
| Body | <500 lines | On skill trigger |
| References | Unlimited | As needed |

### Implications

- Keep body under 500 lines
- Put variant-specific content in references
- Body provides overview + routing to references
- References can be arbitrarily deep

## Reference Organization

When a skill supports multiple variants:

```
skill-name/
├── SKILL.md
└── resources/
    ├── variant-a.md      # Content for variant A
    ├── variant-b.md      # Content for variant B
    └── common.md         # Shared content
```

The body routes to the relevant reference based on context.

## Bundled Resources

**scripts/** — Executable code for deterministic tasks. Can execute without loading into context.

**resources/** — Documentation loaded as needed. For files >300 lines, include a table of contents.

**assets/** — Files used in output (templates, icons, fonts). Referenced but not loaded.

## Example

```markdown
---
name: review
description: >
  Methodology for adversarial code review. Load when reviewing
  code, designs, or plans. Covers severity classification,
  finding format, and review focus areas.
---

# Review Methodology

## Severity Levels

- **Blocking** — Must fix before merge
- **Substantive** — Should fix, can defer with reason  
- **Minor** — Consider fixing

## Finding Format

For each finding: what's wrong, why it matters, fix direction, severity.

## Focus Areas

See `resources/focus-areas.md` for detailed guidance on:
- Correctness review
- Security review
- Performance review
- Design alignment review
```
