# Prompt-Level Principles

How to write the text of a prompt. These apply to agent bodies, skill content, and any system prompt.

## Primacy and Recency

Attention follows a U-curve: strong at the beginning, weak in the middle, strong at the end. This is documented in "Lost in the Middle" research — LLMs retrieve information worse when it's positioned in the middle of context.

**Implications:**
- Open with what this agent does and why it matters. This survives compaction and gets strongest attention.
- Put behavioral constraints with reasoning early. Middle placement loses them.
- Close with critical reminders, completion criteria, or terminal report requirements.
- If something really matters, state it at the beginning AND restate (not copy-paste) near the end.

## Structure Over Emphasis

XML tags, markdown headers, and clear section delineation consistently outperform unstructured prompts. The specific claim that ALL CAPS or "MUST" improves compliance is folklore — no rigorous A/B study exists.

**What works:**
- Consistent section headers
- XML tags for distinct blocks (e.g., `<do_not_act_before_instructions>`)
- Bullet lists for parallel items
- Code blocks for examples

**What's unvalidated:**
- ALL CAPS for emphasis
- "MUST" / "NEVER" / "CRITICAL" markers
- Exclamation points or aggressive tone

If everything is emphasized, nothing is. Reserve strong language for the one or two constraints that truly can't bend.

## Positive Framing

Tell the model what TO do, not what to avoid. Negative constraints are weaker than positive guidance.

**Weak:** "Don't write verbose code. Never add unnecessary comments. Avoid premature abstraction."

**Strong:** "Write concise code. Add comments only when the why is non-obvious. Wait for the third use case before abstracting."

When you must constrain, explain why — that transforms a negative rule into transferable understanding. But default to positive: describe the behavior you want, not the behavior you're prohibiting.

Negative language also has a cognitive cost: the model has to infer what you DO want from what you said not to do. Positive framing is direct.

## Explain Why

The model generalizes from explanations, not rules. Understanding *why* a rule exists lets it apply the principle to novel situations.

**Weak:** "Never edit source files."

**Strong:** "Stay at orchestration altitude — editing source files yourself means you're doing implementation work, which costs you the vantage point to notice when work drifts from user intent."

Expect to write a sentence of reasoning for nearly every constraint. If you can't explain why, the constraint probably shouldn't be there.

## Right Altitude

Too specific: brittle if-then rules that break on edge cases the author didn't anticipate.

Too vague: high-level hand-waving that the model interprets with defaults you didn't want.

**Right altitude:** Tell the model what to do, when, and why. Trust it to apply the principle.

**Example at right altitude:**
> Prefer git diff over reading full files when reviewing a change, because the diff already shows exactly what moved.

## Don't Repeat Across Levels

Three places information can live:
1. Frontmatter description — what the agent does, for callers deciding whether to spawn
2. Body — the system prompt the agent sees
3. Loaded skills — shared reference material

Each level should add new information. If the description says "reviews code for correctness," the body shouldn't repeat that — it should start where the description left off.

Repetition wastes tokens and creates maintenance drift: update one place, forget another, and now the agent gets conflicting instructions.
