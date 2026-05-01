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

Phrases like "the @product-manager spawns you with --from" are meaningless to the receiving model and create false dependency on a specific caller.

**Benefit:** Caller-agnostic agents are reusable across workflows. The same @reviewer can be called from a manager, a CI pipeline, or a human ad-hoc spawn.

## Descriptions Serve Callers

The description is the only thing a caller sees before deciding to spawn. It teaches usage, not just purpose.

Cover:
- **When to use it** — what situation triggers reaching for this agent
- **How to invoke it** — spawn command, typical flags
- **What to pass** — files, context, conversation history
- **How to prompt it** — what to tell it (task scope, constraints, ownership boundaries)
- **What to expect** — output format, where results land

A description that says "reviews code" tells you what it is. A description that says "use when code needs adversarial review, spawn with X, pass artifacts with -f, specify focus area in the prompt for a targeted lane" teaches you how to use it.

## Route by Cognitive Mode

Decompose agents by the type of thinking required, not by file type or domain.

**Instead of:** @backend-coder and @frontend-coder (domain split)

**Think:** @coder (faithful execution against a clear spec) and @frontend-coder (aesthetic judgment against a visual target)

The cognitive mode determines what the agent needs:
- **Faithful execution** — clear spec, concrete deliverable, measured against requirements
- **Aesthetic judgment** — visual target, design fidelity, "does this match?"
- **Ambiguity handling** — unclear requirements, needs to push back and clarify

When routing is by domain, agents fight over boundary cases ("is this React file frontend or backend?"). When routing is by cognitive mode, the question becomes "does this task need aesthetic judgment?" — clearer and more stable.

### Discovering Cognitive Modes

The principle says "route by cognitive mode" — but how do you find the modes for a new domain? Research, not intuition.

Before designing a multi-agent system, investigate how the domain's work is actually done:

1. **Ask the user** — they're the domain expert. How do they do this work? Where do they shift mental stance? What feels like a different "mode" when they do it? A writer knows that brainstorming and editing feel completely different. A developer knows that designing and implementing use different parts of their brain. The user's workflow is the primary source.

2. **Research the domain** — search for how professionals structure this work. Academic papers on creative workflows, team structures in software engineering, editorial pipelines in publishing. The real-world decomposition reveals cognitive mode boundaries that aren't obvious from the outside.

3. **Solo vs team** — identify where one person shifts stance (novelist brainstorming vs outlining), then study how teams decompose the same work (author + developmental editor + copy editor + continuity editor). Where the solo practitioner shifts stance maps to where teams put person-boundaries. That intersection is where agent boundaries belong.

4. **Collapse test** — if you merged two proposed agents back into one, would it need to context-switch between incompatible evaluation criteria? Brainstorming's "explore everything" vs outlining's "commit to structure" are incompatible stances — holding both in one context window degrades both. If collapsing doesn't create conflict, the split isn't justified.

Tasks that one person can do alone are often the *best* candidates for multi-agent decomposition — the cognitive mode shifts are already there, just implicit. Making them explicit puts fresh attention budget on each mode.

## Generic Over Specialized

If an agent's specialization lives entirely in its prompt, it doesn't need to be a separate agent — make the existing agent generic and let the caller's prompt provide the specialization.

**Signs of over-specialization:**
- The new agent's body is nearly identical to an existing one
- The only difference is the task description
- You can't name a distinct cognitive mode or toolset the specialist needs

A generic @browser agent whose caller's prompt defines the purpose (scraping, research, annotation) is more reusable than @design-researcher, @css-scraper, and @site-analyzer as separate agents.
