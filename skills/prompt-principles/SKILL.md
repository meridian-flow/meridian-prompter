---
name: prompt-principles
description: >
  Research-backed principles for designing LLM prompts, agents, and skills.
  Load when writing, reviewing, or improving any prompt artifact. Covers
  prompt-level craft, skill extraction, agent design, and multi-agent
  coordination patterns.
---

# Prompt Principles

Principles for writing effective prompts. See `resources/research.md` for citations where available.

Four levels of prompt design, each with distinct concerns. Load the relevant reference when working at that level.

## Prompt-Level

How to write the text of a prompt. Attention is finite.

- **Be concise, expand for emphasis** — Default to short what and why. When something matters, repeat it or explain it more.
- **Primacy and recency** — Beginning and end get strongest attention; middle gets lost. Put purpose and constraints up front, critical reminders at the end.
- **Structure over emphasis** — XML tags, headers, and clear sections outperform ALL CAPS and "MUST".
- **Positive framing** — Tell the model what TO do, not what to avoid. Positive language directs attention to target behavior; negative instructions keep prohibited behavior in attention and often produce acknowledgments ("we won't do X") instead of omission.
- **Hard boundaries are exceptions** — Negatives work for bright-line prohibitions on protected resources: "Don't modify .agents/" or "Never commit secrets."
- **Explain why** — Reasoning transfers to novel cases. The model applies principles to new situations when it understands the underlying logic.
- **Right altitude** — Behavioral heuristics, not brittle if-then rules or vague hand-waving. Tell the model what to do, when, and why — trust it to sequence.
- **Repetition improves compliance** — Restate key principles at opening and closing of the same artifact. Keep repetition within artifacts; skills are already loaded into context.

See `resources/prompt-level.md` for detail.

## Skill-Level

When to extract shared knowledge into a skill vs keeping it in an agent body.

- **Reuse threshold** — If 2+ agents need the same knowledge, extract to a skill. If only one agent uses it, keep it in the body.
- **Progressive disclosure** — Metadata (~100 words) always loaded; body (<500 lines) on trigger; bundled resources as needed.
- **Skills shape, agents act** — Skills provide knowledge and methodology. They don't run independently or make decisions.
- **Domain organization** — When a skill supports multiple variants (frameworks, platforms), organize by variant with the body routing to the right reference.

See `resources/skill-level.md` for detail.

## Agent-Level

How to design a single agent's role and prompt.

- **Single focus** — Each agent does one job well. Context window is the attention budget; multiple responsibilities compete for it.
- **No role identity** — Skip personas ("you are a senior engineer"). PRISM research shows personas interfere with knowledge retrieval. Describe behavior directly.
- **Context engineering** — A focused window produces better attention than a sprawling one. Fresh spawn = fresh attention budget.
- **Agent owns outputs** — The agent produces artifacts and decisions. Skills inform how; the agent decides what.
- **3-5 function limit** — Single agents handle 3-5 distinct functions before multi-agent coordination helps.

See `resources/agent-level.md` for detail.

## System-Level

How to coordinate multiple agents.

- **Orchestrator pattern** — Coordinator routes and evaluates; workers execute. Orchestrator doesn't implement, workers don't coordinate.
- **Context handoff is caller's job** — Pass structured briefing (objectives, constraints, decisions, evidence), not raw history. ~2% context loss per handoff with naive approaches.
- **External verification required** — Self-critique without external tools (tests, compilers, search) doesn't work. Reviewer must be separate from implementer.
- **Loop guards are external** — The system enforces termination, not the agent. Max iterations (15-25 typical), convergence detection, explicit deferral as valid exit.
- **Start simple** — Default to single agent. Add multi-agent only when evidence shows complexity delivers proportional value.

See `resources/system-level.md` for detail.

