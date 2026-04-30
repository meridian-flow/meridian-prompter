# meridian-prompter

Agents and skills for writing LLM prompts.

## Releasing

Use `mars version` (or `meridian mars version`) for releases:

```bash
mars version patch --push       # 0.0.7 → 0.0.8, commit, tag, push
mars version minor --push       # 0.0.8 → 0.1.0
```

This bumps `mars.toml`, promotes CHANGELOG.md `[Unreleased]` to the new version, commits, tags, and optionally pushes. Write changelog entries under `[Unreleased]` as you work — `mars version` handles the rest.

## Agents

```
agents/
  prompt-dev.md # Collaborative prompt writing sessions
  prompt-reviewer.md       # Reviews prompts against principles
  prompt-tester.md         # Tests agents against sample tasks
  python-tool-writer.md    # Writes Python scripts and tools
  web-prompt-researcher.md # Researches prompting papers and patterns
```

## Skills

```
skills/
  prompt-principles/    # Core prompting principles
  agent-artifacts/      # Agent .md schema
  skill-artifacts/      # Skill schema
  prompt-review/        # Review methodology
  python-tools/         # Python patterns
```

See `skills/prompt-principles/resources/research.md` for citations.
