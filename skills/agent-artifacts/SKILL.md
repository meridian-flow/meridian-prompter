---
name: agent-artifacts
description: >
  Schema and structure for agent definition files — the markdown with YAML
  frontmatter that defines reusable spawn configurations. Load when writing
  or reviewing agent .md files. For meridian-specific patterns, see
  references/meridian.md.
---

# Agent Artifacts

Agent definitions are markdown files with YAML frontmatter under an `agents/` directory.

## File Structure

```
agents/
  agent-name.md
```

Filename matches the `name` field in frontmatter. Use kebab-case.

## Frontmatter Schema

```yaml
---
name: agent-name                    # Required. Matches filename.
description: >                      # Required. Multi-line description.
  What this agent does, how to invoke it, what context it needs,
  where it puts output. Serves callers deciding whether to spawn.
model: opus                         # Optional. Default model (caller can override).
effort: high                        # Optional. Reasoning effort: low, medium, high, xhigh.
skills: [skill-a, skill-b]          # Optional. Skills loaded on launch.
tools: [Bash, Write, Edit]          # Optional. Tool allowlist.
disallowed-tools: [Agent]           # Optional. Tool denylist (enforced on Claude).
sandbox: workspace-write            # Optional. Permission tier.
harness: claude                     # Optional. Usually omit; derived from model.
approval: auto                      # Optional. Approval mode override.
autocompact: 85                     # Optional. Auto-compact threshold.
---
```

### Field Details

**name** — Profile identifier used with `meridian spawn -a <name>`. Match the filename.

**description** — Serves the caller, not the agent. Cover:
- What it does and produces
- How to invoke it (`meridian spawn -a <name>`)
- What context it needs (files, conversation context)
- Where it puts output

**model** — Default model. Options: `opus`, `sonnet`, `haiku`, `gpt`, `codex`, or specific model IDs. Caller can override with `-m`.

**effort** — Reasoning effort tier. Honored by models that support it.

**skills** — Skills loaded on launch. Only list skills needed every invocation.

**tools** — Tool allowlist. Useful for scoping Bash (`Bash(git *)`) and for harnesses like OpenCode/Codex. On Claude, this doesn't restrict built-in tools.

**disallowed-tools** — Tool denylist. On Claude, this removes tools. Use for hard restrictions like `[Agent]` on orchestrators that must use `meridian spawn`.

**sandbox** — Permission tier:
- `read-only` — Can read, can't modify
- `workspace-write` — Can write within repo
- `full-access` — Can touch things outside workspace
- `danger-full-access` — Unrestricted

Match tier to task, not maximum the agent might ever need.

## Body Structure

The body below frontmatter is the system prompt. Structure for attention:

1. **Opening** — What this agent does and why it matters. First thing seen after compaction.

2. **Constraints** — Behavioral boundaries with reasoning. Early placement protects from being lost.

3. **Core workflow** — Inputs, outputs, quality bar, available tools. Usually the bulk.

4. **Specific guidance** — Escalation paths, edge cases, completion criteria. Later because situational.

## Example

```markdown
---
name: reviewer
description: >
  Use when code needs adversarial review. Spawn with
  `meridian spawn -a reviewer`, passing artifacts with -f.
  Read-only — reports findings, doesn't edit.
model: gpt
skills: [review, dev-principles]
tools: [Bash(git diff *), Bash(git log *)]
sandbox: read-only
---

# Reviewer

Primary job: find correctness, regression, and structural risks before they ship.

For each finding:
- what is wrong
- why it matters
- concrete fix direction
- severity
```
