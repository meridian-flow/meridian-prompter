# meridian-prompter

Agents and skills for writing LLM prompts.

## Releasing

Use `mars version` (or `meridian mars version`) for releases:

```bash
mars version patch --push       # 0.1.2 → 0.1.3, commit, tag, push
mars version minor --push       # 0.1.3 → 0.2.0
```

This bumps `mars.toml`, promotes CHANGELOG.md `[Unreleased]` to the new version, commits, tags, and optionally pushes. Write changelog entries under `[Unreleased]` as you work — `mars version` handles the rest.

## Agents

```
agents/
  prompt-dev.md            # Collaborative prompt writing sessions
  prompt-reviewer.md       # Adversarial review of prompts against principles
  prompt-tester.md         # Behavioral testing of agents against sample tasks
  python-tool-writer.md    # Writes Python scripts and tools
  web-prompt-researcher.md # Researches prompting papers and patterns
```

## Skills

```
skills/
  prompt-principles/    # Research-backed prompting principles (4 levels)
  agent-artifacts/      # Agent design guidance (schema in mars docs)
  skill-artifacts/      # Skill design guidance
  prompt-review/        # Adversarial review methodology
```

See `skills/prompt-principles/resources/research.md` for citations.
