# Agent-Level Principles

How to design a single agent's role and prompt.

## Single Focus

Each agent does one job well. The context window is the attention budget — multiple responsibilities compete for it.

**Why this works:**
- Attention is finite within a context window
- Multiple responsibilities dilute focus
- A coder also trying to review its own work splits focus AND creates conflict of interest
- Fresh spawn = fresh attention budget fully focused on one job

Single agents handle 3-5 distinct functions before multi-agent coordination helps.

**Signs an agent is doing too much:**
- The prompt has multiple "## Mode" sections for different behaviors
- The agent needs to switch between creating and evaluating
- The body exceeds 150 lines trying to cover everything
- You're tempted to add "if doing X, then... if doing Y, then..."

## No Role Identity

Don't assign personas like "you are a senior engineer" or "you are a careful reviewer."

The PRISM persona study (arxiv:2603.18507) found that persona prompting activates instruction-following machinery that interferes with knowledge retrieval and reasoning accuracy. On discriminative tasks (judgment calls, factual recall, code reasoning), every persona variant reduced accuracy.

**Instead of persona, describe behavior directly:**

**Weak:** "You are a meticulous code reviewer."

**Strong:** "Focus on correctness: does the code do what it claims? Check edge cases, error handling, and assumptions that could silently break."

The model doesn't need a costume to follow behavioral instructions.

## Context Engineering

A focused window produces better attention than a sprawling one.

**Mechanisms:**
- Single focus per agent (see above)
- Fresh spawn = fresh context, no accumulated cruft
- Pass targeted context, not everything
- Let skills provide stable reference; agent body provides role-specific behavior

**The handoff discipline:** Don't pass raw conversation history. Pass structured briefing: objectives, constraints, prior decisions, relevant evidence. The receiving agent needs to understand the problem, not relive the discussion.

## Agent Owns Outputs

The agent produces artifacts and decisions. Skills inform how; the agent decides what.

**Agent responsibilities:**
- Reading inputs and context
- Making judgment calls
- Producing outputs (code, artifacts, reports)
- Deciding when to escalate or terminate

**Skill responsibilities:**
- Providing methodology
- Supplying reference information
- Offering patterns and examples
- Defining quality criteria

If you're putting decision-making into a skill, it probably wants to be its own agent.

## Caller-Agnostic Bodies

The body shouldn't reference who spawned the agent or how. The agent sees whatever context landed in its window and has no way to distinguish source.

Phrases like "the @dev-orchestrator spawns you with --from" are meaningless to the receiving model and create false dependency on a specific caller.

**Benefit:** Caller-agnostic agents are reusable across workflows. The same @reviewer can be called from a dev orchestrator, a CI pipeline, or a human ad-hoc spawn.
