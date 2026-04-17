# System-Level Principles

How to coordinate multiple agents.

## Orchestrator Pattern

Empirically validated: Anthropic's multi-agent system with orchestrator outperformed single-agent by 90.2% on breadth-first queries.

**Structure:**
- **Orchestrator** — Routes tasks, evaluates outputs, manages convergence. Doesn't implement.
- **Workers** — Execute focused tasks. Don't coordinate with each other directly.

**Why it works:**
- No single agent needs full context
- Orchestrator holds high-level objectives and summaries
- Workers hold only specific subtask input
- Fresh context per worker = focused attention

**The altitude rule:** If you drop into implementation, you lose the vantage point to notice drift. Orchestrators stay at coordination altitude.

## Context Handoff Is Caller's Job

Research shows ~2% context retention loss per handoff step with naive approaches. Structured briefing achieves 91% lower latency and >90% token cost reduction vs full-context.

**Pass:**
- Objectives — what needs to be accomplished
- Constraints — boundaries, requirements, non-goals
- Prior decisions — what's already been decided and why
- Relevant evidence — code pointers, artifacts, findings

**Don't pass:**
- Raw conversation history
- Exploratory discussion that led to the decision
- Context the receiving agent will never use

**The mandate:** The caller gathers context before spawning. The callee shouldn't have to rediscover what the caller already knows.

## External Verification Required

Critical research finding:
> "Almost every published 'success' of reflection is actually a success of verification. Strip away the external tool—the compiler, the test suite, the search engine—and the gains vanish."

**Implications:**
- Self-critique without external tools doesn't work
- Reviewer must be separate from implementer
- Verification needs machine-checkable evidence (tests pass, build succeeds, linter clean)
- "Looks good to me" from the same model that wrote it is not verification

**Patterns that work:**
- Coder → separate Reviewer (different agent, ideally different model)
- Implementation → Tests → Fix loop
- Final review fans out across model families so blind spots don't overlap

## Loop Guards Are External

> "LLMs are notoriously bad at knowing when to stop—they'll either quit too early or never quit at all."

**The principle:** The system enforces termination, not the agent. Loop guardrails are external enforcement.

**Production-validated limits:**
- Max iterations: 15-25 steps typical
- Wall-clock timeout: ~300 seconds for most tasks
- Convergence detection: >85% similar observations/actions across iterations
- Repetition detection: same tool + same arguments called multiple times

**Valid exit conditions:**
- Convergent: no new substantive findings
- Verified: machine-checkable tests confirm success
- Explicitly deferred: remaining items logged in decisions.md
- Escalated: problem is outside agent's visibility, needs human input

## Start Simple

> "Default to a single agent and only introduce multi-agent architecture when you have evidence that additional complexity delivers proportional value."

**The overhead is real:**
- Latency accumulates at each coordination level
- Context loss at each handoff
- More complex debugging
- Higher token costs

**Rule of thumb:** Single agent can handle 3-5 distinct functions. Beyond that, consider splitting.

**When multi-agent is justified:**
- Tasks genuinely require different capabilities (coding vs reviewing)
- Context would overflow a single window
- Parallel execution provides real speedup
- Separation of concerns prevents conflict of interest

**When it's not justified:**
- "Feels cleaner" architecturally
- Following a pattern you saw somewhere
- The single agent is working fine but you want to refactor

## Antipatterns

| Antipattern | Problem |
|-------------|---------|
| **Over-engineering** | Multi-agent when single-agent suffices. Creates hard-to-debug handoffs. |
| **Tool overload** | >20 tools degrades selection quality. Scope the toolset to what the agent actually needs. |
| **Monolithic prompts** | One enormous prompt encoding all behavior. Conflicting instructions, lost coherence. |
| **Task overload** | Too many distinct tasks in single request. Leads to hallucination, missing tasks, low quality. |
| **Self-review** | Same agent reviews its own work. Conflict of interest, no external verification. |
| **Raw history handoff** | Passing full conversation instead of structured briefing. Context bloat, attention dilution. |
