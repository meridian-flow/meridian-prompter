# Prompt-Level Principles

## Be Concise, Expand for Emphasis

Default to short prompts — every token competes for attention. When something truly matters, repeat it or explain it more. Concise baseline, strategic expansion.

## Primacy and Recency

Attention follows a U-curve: strong at beginning and end, weak in the middle.

- Open with purpose and why it matters
- Put constraints with reasoning early
- Close with critical reminders
- Restate key principles at opening and closing

## Structure Over Emphasis

XML tags, headers, and clear sections outperform ALL CAPS and "MUST".

Use: section headers, XML tags, bullet lists, code blocks.

## Positive Framing

State what TO do. Positive language directs attention to target behavior.

**Instead of:** "Don't write verbose code."
**Write:** "Write concise code."

**Why:** Negative instructions keep prohibited behavior in attention. The model acknowledges the prohibition ("we won't do X") instead of simply omitting X.

**Exception:** Hard boundaries on protected resources ("Never commit secrets").

## Explain Why

Attach reasoning so the model can generalize. "Stay at orchestration altitude — delegate to workers for visibility" beats "Never edit source files."

## Right Altitude

Tell the model what to do, when, and why. Trust it to apply the principle.

**Example:**
> Prefer git diff over reading full files when reviewing, because the diff shows exactly what moved.

## Information Layering

1. **Description** — for callers deciding whether to spawn
2. **Body** — system prompt the agent sees
3. **Skills** — shared reference material

Each level adds new information. Restate within levels for emphasis; avoid duplicating across levels.

## Escape Hatches Get Used

"Or lighter context" becomes "always skip context." Optional easier paths become de facto defaults — models prefer the path of least resistance.

If the hard path is the right path, don't offer an easier one. If flexibility is genuinely needed, make the thorough option the default and require explicit justification for shortcuts.

**Example:**
> **Weak:** "Spawned by @dev-orchestrator after design approval, or by @impl-orchestrator when no plan exists."
>
> The second clause is an escape hatch — @impl-orchestrator will always skip design approval.
>
> **Strong:** "Spawned by @dev-orchestrator after design approval, or by @impl-orchestrator to adjust an existing plan mid-flight."
>
> Both paths require prior work. No free shortcut.

## Every Word Carries Decision Weight

Filler weakens prompts. Each word should change what the model does — if removing it doesn't change behavior, cut it.

**Example:**
> "Scoped, behavior-preserving structural refactor" → "behavior-preserving refactor"
>
> "Scoped" adds nothing when the task is already scoped by the caller. "Structural" adds nothing when refactoring is inherently structural.
