# meridian-prompter

A workflow for creating and improving LLM agents and skills from first principles.

## Structure

```
agents/
  prompter-orchestrator.md # Entry point — routes write/review/test
  agent-writer.md         # Drafts agent definitions
  agent-reviewer.md       # Reviews against principles
  agent-tester.md         # Tests agents against sample tasks
  skill-writer.md         # Drafts skill definitions
  skill-reviewer.md       # Reviews skills

skills/
  agent-principles/       # Research-backed design principles
  agent-artifacts/        # Agent .md schema
  skill-artifacts/        # Skill schema
```

## Core Principles

The `agent-principles` skill encodes research-backed principles at four levels:

- **Prompt-level** — Primacy/recency, structure over emphasis, explain why
- **Skill-level** — Progressive disclosure, reuse threshold, skills shape behavior
- **Agent-level** — Single focus, no personas, context engineering
- **System-level** — Orchestrator pattern, external verification, loop guards

See `skills/agent-principles/references/research.md` for citations and the distinction between empirically validated findings and practitioner folklore.
