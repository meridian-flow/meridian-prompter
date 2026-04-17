# Meridian Integration

Reference for writing agents that run in the meridian harness.

## Spawn Syntax

Agents are invoked with:

```bash
meridian spawn -a <agent-name> -p "<prompt>" -f <context-files>
```

Include this pattern in the agent's description so callers know how to invoke it:

```yaml
description: >
  Use when X needs Y. Spawn with `meridian spawn -a agent-name`,
  passing context with -f and requirements in the prompt.
```

### Common Flags

- `-a <name>` — Agent to spawn (matches `name` in frontmatter)
- `-p "<prompt>"` — The task prompt
- `-f <file>` — Context files (can repeat)
- `--from` — Pass conversation context from current session
- `-m <model>` — Override the agent's default model
- `--desc "<text>"` — Short description for task tracking

## Base Skills

Common skills available in meridian-base that agents can load:

| Skill | Purpose |
|-------|---------|
| `meridian-spawn` | Spawn mechanics, flags, background execution |
| `meridian-cli` | Mental model for the CLI |
| `meridian-work-coordination` | Work item lifecycle, status tracking |
| `decision-log` | Recording decisions with reasoning |
| `context-handoffs` | What context to pass between spawns |
| `shared-workspace` | Shared filesystem conventions |
| `review` | Review methodology, severity levels |
| `dev-principles` | Development operating principles |

Reference these in the agent's `skills:` frontmatter when relevant.

## Frontmatter Fields

Meridian-specific fields beyond the base schema:

```yaml
harness: claude              # Usually omit; derived from model
approval: auto               # auto, confirm, yolo
autocompact: 85              # Auto-compact at this % of context
```

### Sandbox Tiers

```yaml
sandbox: read-only           # Analysis, review — can't modify
sandbox: workspace-write     # Implementation — can write in repo
sandbox: full-access         # Needs to touch outside workspace
sandbox: danger-full-access  # Unrestricted
```

## Disallowed Tools Pattern

Orchestrators that delegate via `meridian spawn` should disallow the built-in Agent tool:

```yaml
disallowed-tools: [Agent]
```

This ensures spawns go through meridian's tracking, not the harness's native agent tool.

## Description Pattern

Good meridian agent descriptions include:

1. **When to use** — What problem this solves
2. **How to invoke** — `meridian spawn -a <name>` with typical flags
3. **What context it needs** — Files, conversation context, requirements
4. **Where output goes** — Artifacts location if applicable

Example:
```yaml
description: >
  Use when code needs adversarial review. Spawn with
  `meridian spawn -a reviewer`, passing artifacts with -f
  and session context with --from. Read-only — reports
  findings to terminal, doesn't edit files.
```
