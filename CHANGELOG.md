# Changelog

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning: [SemVer](https://semver.org/).

## [Unreleased]

## [0.0.1] - 2026-04-17

### Added
- Initial release of meridian-prompter — workflow for creating and improving LLM agents and skills from first principles.
- `@prompter-orchestrator`: user-facing entry point with first principles problem breakdown, routes to writers/reviewers/testers.
- `@agent-writer`: drafts agent definitions from requirements, applies `/agent-principles`.
- `@skill-writer`: drafts skill definitions with progressive disclosure structure.
- `@tool-writer`: creates Python scripts for scientific/data analysis, receives research from orchestrator.
- `@agent-reviewer`: adversarial review of agents against principles.
- `@skill-reviewer`: review of skill definitions.
- `@agent-tester`: behavioral testing of agents.
- `agent-principles` skill: research-backed 4-level framework (prompt, skill, agent, system).
- `agent-artifacts` skill: frontmatter schema and artifact conventions.
- `skill-artifacts` skill: skill structure and conventions.
- `review` skill: local review methodology (does not depend on dev-workflow).
- Dependencies: `meridian-base` (spawn, cli), `meridian-dev-workflow` (internet-researcher).
