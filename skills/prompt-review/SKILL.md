---
name: prompt-review
description: >
  Adversarial review methodology for agents and skills. Covers the mindset,
  finding quality, severity handling, and structured reporting. Load when
  reviewing agent definitions or skill content.
---

# Prompt Review

Your job is to find what's wrong, not confirm what's right.

The writer already believes their agent works. Your value comes from challenging that assumption — finding the principle violation they didn't notice, the ambiguity that will confuse the model, the scope creep that dilutes focus. A review that says "looks good" without digging creates false confidence.

## What Makes a Good Finding

- **Specific** — Reference the line or section. "This agent has issues" is not a finding.
- **Reasoned** — Explain why it matters, not just that it exists. A missing constraint is only interesting if you can describe the failure mode.
- **Actionable** — The writer should know what to do after reading your finding.
- **Non-obvious** — You're here for things that require understanding principles and intent, not typos.

### What wastes time

- Vague "this could be better" without explaining how
- Style nitpicks that don't affect behavior
- Restating what the prompt says without identifying a problem

## Severity

Make it obvious which findings matter:

- **Blocking** — Must fix. Violates core principles, creates real failure modes.
- **Substantive** — Should fix. Weakens the design, causes confusion, reduces reusability.
- **Minor** — Consider fixing. Polish, clarity improvements, nice-to-haves.

Lead with blocking issues. Whoever triages your findings can downgrade severity, but they can't act on buried problems.

## The Adversarial Mindset

Think about how the agent fails, not how it succeeds:

- What if the input is ambiguous?
- What if the agent is spawned without expected context?
- What if this conflicts with a skill it loads?
- What principles does this violate and why does that matter?
- Does the prompt use positive framing? Flag negative instructions that keep prohibited behavior in attention.

Don't be adversarial for its own sake. The goal is finding real problems, not demonstrating cleverness. If the agent is genuinely good, say so — but earn that conclusion by actually looking.

## Report Structure

1. Brief overall assessment
2. Findings grouped by severity or theme
3. Verdict: approve, approve with notes, or request changes

If requesting changes, be clear about which findings are blocking.
