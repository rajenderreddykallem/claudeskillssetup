---
description: Turn a raw idea, one-line feature request, or ticket into a structured technical spec before any design or code happens. Use when the user has a rough idea, pastes a vague ticket, or asks "how should I build X" / "help me spec this out" — before jumping to architecture or implementation.
---

# Idea to Spec

Convert ambiguity into a written spec the user can hand to another engineer without re-explaining it verbally. A 20-year engineer's biggest lever here isn't typing speed — it's refusing to let ambiguity survive into code.

## Rules

- Do not start designing or coding from a one-line request. Produce the spec first.
- If a required fact is missing (who the user is, what "done" means, what's explicitly out of scope), ask — don't invent a plausible-sounding answer and move on. One round of targeted questions beats three rounds of rework.
- Prefer a short spec that's actually read over a thorough one that isn't. Cut anything that doesn't change a decision.

## Spec structure

Produce markdown with these sections, skipping any that are genuinely not applicable (say why):

1. **Problem** — one paragraph: what's broken or missing, for whom, and why now. Not "add a feature," but the pain it removes.
2. **Goals** — bullet list, each independently verifiable.
3. **Non-goals** — explicit. This is the section that prevents scope creep; write it as carefully as goals.
4. **Constraints** — technical (existing systems this must integrate with), organizational (deadline, team size), compliance/security if relevant.
5. **Proposed approach (sketch only)** — one paragraph or a rough flow, enough to validate direction. Full design belongs in an ADR (see `architecture-decision` skill) or implementation plan, not here.
6. **Success metric** — how you'll know this worked after shipping. If there isn't a measurable one, say what observable behavior changes.
7. **Assumptions & open questions** — everything you guessed at or couldn't resolve. This section is the point of the exercise; a spec with no open questions was probably rubber-stamped, not thought through.
8. **Rough size** — S / M / L, with the single biggest source of uncertainty named.

## Output

Write the spec to a file (e.g. `docs/specs/<slug>.md`) rather than only in chat, so it survives as a reference. If the user is working from a GitHub issue, offer to post it there instead.
