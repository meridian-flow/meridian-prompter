# Changelog

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning: [SemVer](https://semver.org/).

## [Unreleased]

### Changed
- **prompt-principles**: Added "Be concise, expand for emphasis" as core principle
- **prompt-principles**: Rewrote "repetition" principle — repetition improves compliance within artifacts
- **prompt-principles**: Applied positive framing throughout, removed negative explanations
- **prompt-principles**: Removed folklore section from research.md and split direct empirical support from operational guidance
- **prompt-principles**: Tightened prompt-level.md for token efficiency
- **prompt-review**: Added positive framing check to adversarial review
- **AGENTS.md**: Rewrote to reflect actual agent/skill structure

### Added
- **prompter-orchestrator**: Renamed from prompt-writer, rewritten with dev-orchestrator style — collaborative sessions, active engagement, iterative drafting
- **web-prompt-researcher**: New agent for researching prompting papers and patterns

### Removed
- Dependency on `meridian-dev-workflow` (was only used for internet-researcher)

### Fixed
- Consistent use of `resources/` directory name (was mixed with `references/`)
- Removed false "empirical" claims for practitioner heuristics (3-5 function limit)

## [0.0.2] - 2026-04-17

### Changed
- Renamed `review` skill to `prompt-review` to avoid collision with `meridian-dev-workflow`'s review skill.

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
