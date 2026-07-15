---
description: Assess whether a change is ready to ship to production and produce a rollout/deployment plan. Use when the user asks "is this ready to deploy," wants a rollout plan, or is about to merge a significant change to a production system.
---

# Deployment Readiness

A change is not "done" when the code is correct — it's done when there's a safe way to get it into production and a safe way to get it back out if it's wrong.

## Checklist

Walk through each item and give a concrete answer, not a checkbox. "N/A" is acceptable only with a one-line reason.

- **Rollout strategy** — feature flag, canary, blue-green, or big-bang. Pick the least risky option that's practical for this change; state why a safer option isn't being used if a riskier one is chosen.
- **Rollback plan** — write the exact rollback steps now, before shipping, not after something breaks. Is it a fast revert (flag flip, redeploy previous version) or does it also require a data/migration rollback? If a migration is involved, is it backward compatible during the rollout window (can old code run against the new schema, and vice versa)?
- **Observability** — what specific metric, log line, or alert would tell you within minutes that this broke, as opposed to being discovered by a user report days later? If there's no such signal, that's a gap to close before shipping, not after.
- **Compatibility during rollout** — for API/schema/message-format changes, can the old and new versions coexist while the rollout is in progress (multiple instances running different versions is the normal case, not the edge case)?
- **Capacity/load impact** — does this change resource usage meaningfully (new queries, new background jobs, increased payload size)? Does anything need pre-warming or a scaling adjustment beforehand?
- **Blast radius** — what's the worst realistic outcome if this is wrong, which users/systems are affected, and how fast can it be contained?
- **Dependencies** — what upstream/downstream systems or teams does this affect, and who needs a heads-up before it ships?
- **On-call readiness** — if this triggers a page, does a runbook exist for it (see `write-docs` skill), or is the on-call engineer starting from zero?

## Output

End with an explicit **go / no-go** and, if no-go, the specific blocking items — not a vague "looks mostly ready." If go, restate the rollback command/steps one more time at the end so it's the last thing in view before shipping.
