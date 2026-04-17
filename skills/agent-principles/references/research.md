# Research Backing

What's empirically validated vs practitioner folklore.

## Empirically Validated

| Finding | Source |
|---------|--------|
| Position matters (primacy/recency) | "Lost in the Middle" (Liu et al., 2023) |
| Persona prompting hurts reasoning | PRISM study (arxiv:2603.18507) |
| RLHF models follow instructions better than pre-RLHF | InstructGPT (arxiv:2203.02155) |
| Structure (XML tags) beats unstructured prompts | Anthropic prompt engineering docs |
| Multi-agent orchestrator outperforms single on breadth-first queries | Anthropic multi-agent research (90.2% improvement) |
| ~2% context retention loss per handoff step | Multi-Layered Memory Architectures (arxiv:2603.29194) |
| Self-reflection without external tools fails | CRITIC framework (arxiv:2305.11738) |
| Tool set >20 degrades selection quality | Agent harness failures research |
| Max iterations 15-25 typical for production | Multiple production systems documented |
| ReAct outperforms pure CoT on multi-hop reasoning | ReAct (arxiv:2210.03629) |

## Practitioner Folklore (Unvalidated)

| Claim | Status |
|-------|--------|
| ALL CAPS improves compliance | No rigorous A/B study |
| "MUST"/"NEVER" improves compliance | Not specifically tested |
| Emphatic language causes overtriggering on new models | Observation, not studied |
| Emotional prompts help | Mixed evidence; task-dependent |

## Key Quotes from Research

On self-critique:
> "Almost every published 'success' of reflection is actually a success of verification. Strip away the external tool—the compiler, the test suite, the search engine—and the gains vanish."

On multi-agent complexity:
> "Default to a single agent and only introduce multi-agent architecture when you have evidence that additional complexity delivers proportional value."

On task decomposition:
> "In most cases, task decomposition has limited impact: if the task is simple, a single agent can solve it; if it is too difficult, multi-agent systems still fail."

On convergence:
> "LLMs are notoriously bad at knowing when to stop—they'll either quit too early or never quit at all."

On loop guards:
> "Loop guardrails are based on external enforcement: the system running the agent, not the agent itself, is ultimately responsible for guaranteeing termination."

On implementation approach:
> "Most successful implementations weren't using complex frameworks but simple, composable patterns."

## Surveys for Deeper Reading

- Large Language Model Agent: A Survey on Methodology, Applications and Challenges (arxiv:2503.21460)
- Large Language Model based Multi-Agents: A Survey of Progress and Challenges (arxiv:2402.01680)
- Understanding the Planning of LLM Agents: A Survey (arxiv:2402.02716)
- Anthropic: Building Effective Agents (anthropic.com/research/building-effective-agents)
