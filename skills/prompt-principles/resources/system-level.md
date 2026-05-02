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
- Explicitly deferred: remaining items logged with reasoning
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

## Explicit Handoff Content

Name specific artifacts at every handoff boundary. Categories aren't enough — file paths are.

**Weak:** "Based on the design, implement it."

**Strong:** "Implement phase 2 per `design/spec/auth.md`, respecting the integration boundaries in `design/architecture/boundaries.md`."

The weak version pushes synthesis onto the receiving agent — it must figure out which files matter, which decisions apply, and what "the design" even means. The strong version tells it exactly what to read.

At pipeline narrowings, lost context compounds. A 2% loss at each step becomes significant across a 5-step pipeline. Explicit file references are the cheapest insurance against silent scope loss.

## Verify Alignment at Narrowings

Pipelines have an hourglass shape: wide exploration (design) narrows to a single artifact (plan), then widens again (parallel implementation). The narrow point is where scope loss occurs — requirements present in the design silently drop from the plan, and implementation faithfully builds the wrong thing.

Verify alignment at every narrowing:
- Does the plan cover everything the design specified?
- Does the implementation deliver everything the plan promised?
- Are behavioral requirements traceable end-to-end?

Verification at the end catches drift too late. Verification at each transition catches it while context is fresh and fixes are cheap. Dedicated alignment verification (separate from code review) catches coverage gaps that look invisible to the implementer.

## Match Model to Cognitive Mode

Different models excel at different cognitive modes. Matching matters for multi-agent staffing:

- **Clear-goal execution** — models that follow instructions faithfully, execute fast, don't overthink. Best for implementation against clear specs.
- **Ambiguity handling** — models that push back, ask clarifying questions, handle underspecified tasks. Best for orchestration and user-facing roles.
- **Judgment and nuance** — models with strong reasoning and evaluation depth. Best for adversarial review and architectural decisions.

Mismatches are expensive in both directions: an overthinking model on a simple task wastes cost and time; a shallow-execution model on a judgment task produces fast, wrong answers.
