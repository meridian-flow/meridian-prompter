# Meridian Design Principles

Design principles specific to the Meridian multi-agent ecosystem. These extend the core prompt principles with patterns for agents that spawn and coordinate with each other.

## Subagent Design

Agents spawned by orchestrators should be **caller-agnostic** — they work regardless of who spawned them.

**Don't reference the caller.** The body shouldn't mention "the orchestrator spawns you with..." — the agent sees context in its window with no way to know where it came from.

**Assume focused context.** The caller decided what to pass. Work with what arrived. If critical context is missing, flag it rather than guessing.

**Fresh attention budget.** Each spawn starts with a clean context window. This is an advantage — work from the briefing, don't try to reconstruct full history.

**Single job.** Subagents do one thing well. Multiple distinct jobs → multiple agents.

**Produce artifacts, not just responses.** Files survive compaction and can be passed to the next spawn. Discover where to write via the CLI, not hardcoded env vars:

- `meridian work current` — active work directory for this session. Can be empty when no work is attached; fall back to a caller-provided path, or pick `meridian context work` vs `meridian context kb` by artifact scope (work-scoped vs durable).
- `meridian context work` — configured work context root.
- `meridian context work.archive` — archived work root.
- `meridian context kb` — knowledge base root for durable notes.

`MERIDIAN_*` path vars are internal/compatibility propagation, not the user-facing discovery API. When env is unavoidable: `MERIDIAN_PROJECT_DIR` is the project/repo directory override, `MERIDIAN_PROJECT_ROOT` is the runtime state root (not the repo root), and prefer `MERIDIAN_KB_DIR` over the legacy `MERIDIAN_FS_DIR`.

## Context Handoffs

The caller decides what context to pass. Two mechanisms:

**`-f` (files)** — specific files the agent needs. Use when context exists as files, is stable, and inspectable.

**`--from` (conversation context)** — reasoning from a prior spawn. Use when the agent needs decisions/rationale that aren't materialized to files.

- `--from <spawn-id>` — pulls a specific prior spawn's transcript
- `--from $MERIDIAN_CHAT_ID` — pulls the top-level primary session. Available at any spawn depth because the env var is inherited. Use when a descendant needs the original framing, decisions, or user intent.

`--from` supplies framing and rationale; concrete scope still comes from `-f` files and the prompt.

### Handoff Principles

**Name specific files, not categories.** Every `-f` should be a concrete path. "Pass the design docs" is ambiguous. `-f design/spec/auth.md -f design/decisions.md` is actionable.

```bash
# Weak — pushes file discovery onto the agent
meridian spawn -a coder -p "Implement auth based on the design"

# Strong — agent knows exactly what to read
meridian spawn -a coder \
  -p "Implement phase 2 auth per the spec, respecting integration boundaries" \
  -f design/spec/auth.md \
  -f design/decisions.md \
  -f src/middleware/
```

**Folder for structure, files for content.** Pass a directory with `-f` to give the agent a tree listing (map), plus specific files for the content that matters most. The agent sees the structure and reads what it needs.

**Pass overview plus specifics.** The overview orients; specifics tell what to do. 2-4 files is typical. 10 means you're delegating understanding.

**Prove you understood.** Include file paths, key decisions, what specifically to do. "Based on the design, implement it" pushes synthesis onto the agent.

**Materialize critical context.** If context only lives in conversation and losing it would hurt, write it to a file first. Files survive crashes; sessions may compact.

**~2% context loss per naive handoff.** Structured briefings (objectives, constraints, decisions, evidence) lose less than raw history dumps.

### Pipeline Handoffs

At each pipeline transition, handoff quality determines whether scope survives. Every transition is a narrowing where context can silently drop.

**At each transition:**
- Pass the upstream artifact explicitly with `-f` — the receiving agent's job is to execute, not to rediscover what the prior stage decided
- Tell the agent what it owns — scope, behavioral requirements, boundaries to respect
- Pass the source artifact alongside the output artifact to the reviewer — alignment review needs both to compare

The principle: each spawn command should make it obvious what the agent reads and what it's responsible for. If you can't name the `-f` files, the handoff context doesn't exist as artifacts yet — materialize it first.

## Orchestrator Principles

Orchestrators coordinate — they don't implement.

### The Core Pattern

```
Orchestrator (routes, evaluates) → Workers (execute)
```

The orchestrator:
- Captures requirements, confirms direction
- Spawns workers with focused context
- Evaluates results, routes next steps
- Handles convergence and escalation

The orchestrator does NOT:
- Write code or edit files
- Do work it should delegate
- Mix implementation with coordination

### Why This Separation Matters

If the orchestrator implements:
- It loses the vantage point to notice drift
- It can't objectively review its own work
- Spawns don't produce traceable reports
- Review/implementation separation breaks

Even for "trivial" tasks, spawn a worker. The orchestrator's value is coordination altitude.

### Convergence and Loop Guards

Orchestrators control when to stop — workers don't self-terminate loops.

- Define convergence criteria (e.g., "no blocking findings")
- Set max iterations externally (15-25 typical)
- Allow explicit deferral as valid exit
- Escalate to user when loops don't converge

### Handoff Discipline

When spawning workers:
- Pass targeted context, not everything
- Include the specific task and success criteria
- Tell them where to put output
- Brief them — don't assume they have history

## Model Staffing

Match model choice to the agent's cognitive mode. Model defaults belong in agent profiles, not hardcoded in spawn commands — `meridian mars models list` shows the live catalog.

**Clear-goal execution** — fast, instruction-faithful models. Best for coders, testers, builders with clear specs. These models excel when the target is unambiguous; they overthink when it isn't.

**Ambiguity handling** — models that push back, ask questions, iterate with the user. Best for primary/orchestrator agents, design exploration, user-facing roles. These models handle underspecified tasks where the "right answer" emerges through dialogue.

**Judgment and review** — models with evaluation depth and nuanced reasoning. Best for adversarial review, architectural decisions, complex tradeoff analysis.

When an agent profile specifies a model, the profile author already matched model to cognitive mode. Override with `-m` only when you have a specific reason — not as a default.
