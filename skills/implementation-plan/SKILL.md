---
description: Break an approved spec, design, or ADR into an ordered, actionable implementation plan before writing code. Use once direction is settled and it's time to sequence the actual build — "how should I sequence this," "break this down into steps," "what order should I build this in."
---

# Implementation Plan

Sequencing is where senior engineers save the most time: get the order wrong and you discover a blocking unknown after three days of work on the easy parts.

## Sequencing principles

- **Riskiest/most uncertain part first.** If a step could invalidate the whole plan (an API doesn't support what you need, a library has a hard limitation, a perf ceiling exists), do that spike before building anything that depends on it succeeding.
- **Slice vertically, not horizontally.** Prefer "one thin end-to-end path works" over "all the models, then all the endpoints, then all the UI." A vertical slice surfaces integration problems immediately instead of at the end.
- **Identify what ships behind a flag vs. what needs a hard cutover.** Flag-gated work can land incrementally in small reviewable PRs; migrations and format changes usually can't — call these out separately.
- **Migrations and backfills are their own step**, with an explicit rollback note, never folded silently into a feature step.
- **Order by dependency, not by file/layer.** A step should be shippable (or at least mergeable to main without breaking anything) on its own.

## Per-step format

For each step in the plan, state:

- What changes (files/components, at a level useful for review, not a line-by-line diff)
- How it's tested (which test type — unit/integration/manual — and what it proves)
- How it's verified working (what you'd check to confirm it actually works, not just "tests pass")
- Dependencies on earlier steps
- Whether it touches shared/critical infrastructure (auth, billing, data migrations, public APIs) — flag these for extra review, since a mistake there has a wider blast radius than a local change

## Output

A numbered checklist. Keep each step small enough to review in one sitting — if a step description needs its own sub-bullets to explain "what changes," it's probably two steps. Note the total step count and flag the step you're least confident about.
