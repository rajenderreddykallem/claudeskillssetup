---
description: Write or update technical documentation - READMEs, API docs, runbooks, ADR summaries, or PR descriptions. Use when the user asks to document a feature, write a README, explain how something works for other engineers, or prepare a runbook.
---

# Technical Documentation

Write for the reader who's debugging this at 3am with no context, not for the author who already knows it. Every sentence should tell that reader something they didn't already know from the code or the file name.

## Principles

- Lead with **why/what** in one paragraph before **how**: what this is, who it's for, when to reach for it.
- State facts plainly. No filler ("this powerful feature," "simply," "just") — if a step were actually simple, it wouldn't need documenting.
- Keep docs next to the code they describe (same directory or a linked `docs/` path). A doc no one can find is worse than no doc, because it creates false confidence that documentation exists.
- Delete or clearly mark stale docs rather than let them silently drift from the code. A wrong doc is worse than a missing one.

## README skeleton

Purpose (1-2 sentences) → Quickstart (fewest steps to run it) → Configuration/env vars (name, purpose, default) → Architecture (diagram or one-paragraph pointer to where the real design doc lives) → How to run tests → How to deploy → Troubleshooting (the 2-3 things that actually go wrong) → Ownership/contact.

## Runbook skeleton

For operational docs (what to do when an alert fires):

- **Symptom** — exactly what the alert/error looks like
- **Diagnosis** — the specific commands/dashboards to check, in order, with what each result means
- **Mitigation** — the fastest safe action to stop the bleeding (not necessarily the permanent fix)
- **Rollback** — the exact command or process to revert, tested and copy-pasteable
- **Escalation** — who/what to page if mitigation doesn't work, and after how long

## API docs

For each endpoint/method: one concrete example request and response (real values, not `<placeholder>`), the error codes with what each means and when it's returned, auth requirements, and rate limits if any. Skip endpoints that are internal-only implementation details unless someone will actually consume them directly.

## PR descriptions

State the "why" (what problem this solves) over the "what" (the diff already shows what changed). Note anything a reviewer should pay special attention to, and call out what was deliberately left out of scope.
