# Meridian Design Patterns

Design patterns for agents in the Meridian ecosystem. For schema and
configuration, see the [mars docs](https://github.com/meridian-flow/mars-agents/blob/main/docs/config/).

## Subagent Design

Agents spawned by managers or leads should be **caller-agnostic** — they
work regardless of who spawned them.

Assume focused context. The caller decided what to pass — work with what
arrived. If critical context is missing, flag it rather than guessing.

Produce artifacts, not just responses. Files survive compaction and can be
passed to the next spawn. Discover where to write via the CLI:

- `meridian work current` — active work directory
- `meridian context work` — work context root
- `meridian context kb` — knowledge base root

## Context Handoffs

**Name specific files, not categories.** Every `-f` should be a concrete
path. `-f design/spec/auth.md` is actionable; "pass the design docs" is
ambiguous.

**Pass overview plus specifics.** The overview orients; specifics tell what
to do. 2-4 files is typical. 10 means the handoff needs rethinking.

**Materialize critical context.** If context only lives in conversation and
losing it would hurt, write it to a file first.

Use `--from <spawn-id>` for reasoning from a prior spawn, `--from
$MERIDIAN_CHAT_ID` for the primary session's decisions and intent.

## Model Staffing

Match model choice to the agent's cognitive mode. Model defaults belong in
agent profiles — `meridian mars models list` shows the live catalog.

- **Clear-goal execution** — fast, instruction-faithful. Best for coders and builders with clear specs.
- **Ambiguity handling** — pushes back, iterates. Best for managers, leads, design exploration.
- **Judgment and review** — evaluation depth. Best for adversarial review and architectural decisions.
