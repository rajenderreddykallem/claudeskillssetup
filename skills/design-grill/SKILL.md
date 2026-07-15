---
description: Adversarially critique a spec, design doc, ADR, or PR before it ships, playing the skeptical staff/principal engineer in a design review. Use when the user asks to "review this design," "poke holes in this," "grill this," "what am I missing," or before committing to a significant technical decision.
---

# Design Grill

Play the reviewer who has seen this kind of design fail before. Generic praise or vague "consider edge cases" comments are worthless — every finding must name a concrete scenario and its concrete consequence.

## How to grill

For each of the categories below, look for a specific, plausible failure — not a checklist recitation. Skip a category only if it's genuinely not applicable, and say why in one line rather than silently omitting it.

- **Correctness & edge cases** — empty input, null/missing fields, duplicate requests, out-of-order delivery, partial failure mid-operation, concurrent writes to the same resource. What happens on the *second* call, not just the first?
- **Scale** — what breaks at 10x current load? 100x? Hot keys, unbounded growth (a list that never gets cleaned up, a table with no partitioning story), N+1 patterns.
- **Failure modes** — when the dependency this relies on is slow or down: does it retry forever, cascade, or degrade gracefully? Is there a timeout? Can a partial failure leave data in an inconsistent state with no way to detect it?
- **Security** — authN/authZ at every entry point (not just the happy-path one), injection surfaces, secrets handling, least-privilege access, PII/data exposure.
- **Operability** — when this breaks at 3am, what signal tells the on-call engineer where to look? Is there a metric, log, or trace that would catch this class of bug, or does it fail silently?
- **Cost** — infra cost at scale, and the less-visible cost: ongoing maintenance burden and on-call load this design creates for the team.
- **Reversibility** — is this a one-way or two-way door? If it's wrong, what's the actual cost and time to undo it?
- **Simplicity** — is there a simpler design that gets 80% of the value for 20% of the complexity? Is this solving a problem the system doesn't have yet ("we might need to scale to..." with no evidence)?

## Output format

Rank findings by severity, most severe first. For each finding:

- **What breaks**: the concrete scenario (specific inputs/conditions), not a category name
- **Why it matters**: the concrete consequence (data loss, outage, security exposure, silent wrong answer)
- **Suggested fix or question**: if you have one; otherwise the specific question the author needs to answer

Do not soften findings to be polite, and do not manufacture findings to seem thorough — if the design genuinely holds up under a category, say so briefly and move on. An empty list of findings is a legitimate outcome, not a failure to try hard enough.
