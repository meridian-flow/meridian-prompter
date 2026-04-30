# Meridian Agent Schema

Schema and conventions for agents in the Meridian ecosystem. For design principles (subagent design, context handoffs, orchestrator patterns), see `prompt-principles/resources/meridian.md`.

## Spawn Syntax

```bash
meridian spawn -a <agent-name> -p "<prompt>" -f <context-files>
```

### Common Flags

| Flag | Purpose |
|------|---------|
| `-a <name>` | Agent to spawn (matches frontmatter `name`) |
| `-p "<prompt>"` | The task prompt |
| `-f <file>` | Context files (repeatable) |
| `--from <spawn-id>` | Conversation context from prior spawn |
| `-m <model>` | Override default model |
| `--desc "<text>"` | Short description for tracking |
| `--approval <mode>` | Override approval mode |
| `--sandbox <tier>` | Override sandbox tier |

## Description Pattern

The description helps callers decide **when and why** to spawn this agent.

**Lead with the problem it solves:**

```yaml
# Good — tells you when to reach for it
description: >
  Use when code needs adversarial review — correctness, regression risk,
  structural soundness. Spawn with `meridian spawn -a reviewer`, passing
  artifacts with -f. Read-only — reports findings, doesn't edit.

# Bad — mechanics without the "when"
description: >
  Reviews code and produces findings. Takes files as input.
```

Structure:
1. **When/why to use** — What problem? What situation?
2. **How to spawn** — `meridian spawn -a <name>` with typical flags
3. **What context it needs** — Specific files to pass with `-f`, conversation context with `--from`
4. **How to prompt it** — What to tell it in the task prompt: scope, constraints, ownership, focus area
5. **Where output goes** — If applicable

```yaml
# Good — teaches the caller how to use it effectively
description: >
  Use for implementation tasks ready to execute against a phase blueprint.
  Spawn with `meridian spawn -a coder`, passing the blueprint and relevant
  source files with -f. Tell it which subphase to implement, which
  behavioral requirements it owns, and any integration boundaries to respect.

# Weak — tells you what it is but not how to use it
description: >
  Implements code changes based on plans.
```

## Permission Model

### Sandbox Tiers

Match tier to task, not maximum possible:

| Tier | Allows |
|------|--------|
| `read-only` | Analysis, review — can't modify |
| `workspace-write` | Implementation within repo |
| `full-access` | Touch things outside workspace |
| `danger-full-access` | Unrestricted |

### Approval Modes

| Mode | Behavior |
|------|----------|
| `default` | Harness decides |
| `confirm` | User approves each tool call |
| `auto` | Auto-approve safe operations |
| `yolo` | Approve everything |

### Tool Restrictions

- **`tools:`** — Allowlist. Scopes Bash (`Bash(git *)`). On Claude, doesn't restrict built-ins.
- **`disallowed-tools:`** — Denylist. Enforced on Claude. Use `[Agent]` on managers/leads.

## Manager/Lead Frontmatter

```yaml
disallowed-tools: [Agent, Edit, Write, NotebookEdit]
sandbox: danger-full-access
approval: auto
skills: [meridian-spawn, ...]
```

`disallowed-tools: [Agent]` forces use of `meridian spawn`, which provides tracking and reports.

## Source Package Routing

Agents/skills live in source repos, synced to `.agents/` via `meridian mars sync`.

**Never edit `.agents/` directly.**

| Package | Contains |
|---------|----------|
| `meridian-base` | Core infrastructure — spawn skills, CLI skills |
| `meridian-dev-workflow` | Dev agents — coder, reviewer, leads, managers |
| `meridian-prompter` | Prompt agents — prompt-dev, prompt-reviewer, prompt-tester, python-tool-writer, web-prompt-researcher |

Workflow: edit source → commit → `meridian mars sync` → `.agents/` regenerates.

## Base Skills

| Skill | Purpose |
|-------|---------|
| `meridian-spawn` | Spawn mechanics, flags |
| `meridian-cli` | CLI mental model |
| `meridian-work-coordination` | Work item lifecycle |
| `decision-log` | Recording decisions |
| `context-handoffs` | What to pass between spawns |
| `shared-workspace` | Filesystem conventions |
