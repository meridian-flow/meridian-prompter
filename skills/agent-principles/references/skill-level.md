# Skill-Level Principles

When to extract shared knowledge into a skill vs keeping it in an agent body.

## Reuse Threshold

**If 2+ agents need the same knowledge, extract to a skill.**

If only one agent uses it, keep it in the agent's body. Extracting single-use knowledge creates two places to maintain with no reuse benefit.

Signs you need a skill:
- You're copy-pasting the same guidance into multiple agent bodies
- Multiple agents reference the same methodology (e.g., spawn mechanics, artifact layouts)
- Knowledge is stable across agents but agents diverge in behavior

Signs you don't need a skill:
- Only one agent uses this knowledge
- The guidance is tightly coupled to this agent's specific behavior
- Extracting would create a skill that's really "how to be this one agent"

## Progressive Disclosure

Skills use a three-level loading system:

1. **Metadata** (name + description) — Always in context (~100 words). This is the triggering surface.
2. **SKILL.md body** — Loaded when skill triggers (<500 lines ideal). The main content.
3. **Bundled resources** — Loaded as needed (unlimited size). Scripts execute without loading into context.

**Implications:**
- Keep SKILL.md under 500 lines. If approaching this limit, add hierarchy with clear pointers to deeper references.
- Put routing logic in the body, detailed content in references
- For large reference files (>300 lines), include a table of contents

## Skills Shape, Agents Act

Skills are reference material loaded into agents. They don't run independently.

**Agents:**
- Spawned as their own process
- Get their own context window
- Make decisions and produce output
- Run independently

**Skills:**
- Loaded into an agent's context
- Shape the agent's behavior
- Provide knowledge and methodology
- Don't run or decide anything themselves

Getting this wrong is costly:
- Skill where agent belongs → forces a single consumer to become a god-object
- Agent where skill belongs → spawns a whole process just to deliver reference material

## Domain Organization

When a skill supports multiple variants (frameworks, platforms, providers), organize by variant:

```
cloud-deploy/
├── SKILL.md (workflow + selection logic)
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```

The body routes to the relevant reference; the agent reads only what applies. This keeps context focused and avoids loading irrelevant variants.
