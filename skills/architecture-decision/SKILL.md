---
description: Produce an Architecture Decision Record (ADR) for a consequential technical choice — database, service boundary, protocol, sync vs async, build vs buy, library choice. Use when the user asks "which approach should I use," needs to justify a design decision, or a choice needs to be remembered and revisited later.
---

# Architecture Decision Record

A decision without written alternatives is a guess with confidence. The point of an ADR isn't the document — it's forcing at least two real options onto the table before committing.

## Rules

- Always produce **at least two genuine alternatives**, not a strawman and the "obviously correct" answer. If you can only think of one real option, say so explicitly and explain why the space is that constrained — don't manufacture a weak second option to look thorough.
- State whether this is a **one-way door or a two-way door** (Amazon's framing): can this be cheaply reversed if wrong, or does it lock in data formats / public contracts / vendor commitments? One-way doors deserve more scrutiny and a slower decision; two-way doors deserve a fast decision and a plan to revisit.
- Be honest about what the decision gives up, not just what it gains. Every real tradeoff costs something — name it.

## ADR structure

Write to `docs/adr/NNNN-<slug>.md` (sequential number, check the directory for the next available one):

```markdown
# NNNN. <Title>

**Status:** proposed | accepted | superseded by NNNN
**Date:** YYYY-MM-DD

## Context
What forces are at play — technical constraints, team constraints, timeline. What
makes this decision non-obvious.

## Options considered
| Option | Complexity | Perf/scale | Team familiarity | Reversibility | Ops burden |
|---|---|---|---|---|---|
| A | | | | | |
| B | | | | | |
| C (if applicable) | | | | | |

Brief prose on each option's real-world failure mode, not just the table.

## Decision
Which option, and the one or two factors that actually tipped it — not a
restatement of the whole table.

## Consequences
- What we're accepting (the cost of this choice)
- What we're explicitly deferring or ruling out
- **Revisit when:** the concrete trigger condition that should prompt reopening
  this decision (e.g. "if p99 latency exceeds Xms" or "if team grows past Y")
```

## Output

Save the file, then summarize the decision and its reversibility in 2-3 sentences in chat — don't make the user open the file to know what was decided.
