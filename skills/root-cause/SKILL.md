---
description: Systematically debug a failure, bug, incident, or unexpected behavior instead of guessing at fixes. Use when the user reports something broken, pastes an error or stack trace, or a test/feature isn't behaving as expected.
---

# Root Cause Debugging

Guessing-and-checking on a live bug burns far more time than it saves. Work the problem in this order.

## Method

1. **Reproduce first.** Get the smallest reliable repro before touching any code. If it can't be reproduced, say so explicitly and treat the investigation as evidence-gathering (logs, timing, affected scope) rather than trial-and-error fixing.
2. **Read the actual evidence fully** before theorizing — the complete stack trace, not just the top line; the actual error message text; what changed recently (recent diff, config, dependency version, deployment). Check the obvious before the exotic.
3. **Form a hypothesis, then state what you'd expect to observe if it's true**, before changing code. If the observation doesn't match, the hypothesis was wrong — don't quietly pivot to "fixing" something adjacent and hope it helps.
4. **Bisect to localize.** Binary search over time (`git bisect`), over input space (which input triggers it, which doesn't), or over code path (comment out / short-circuit sections) to narrow down where the fault actually lives, rather than staring at the whole system.
5. **Distinguish trigger from root cause.** The proximate line that threw is rarely the root cause. Ask "why" repeatedly until you reach a systemic reason (missing validation, an assumption that stopped holding, a race condition) — not just "because this variable was null there."
6. **Write a regression test that encodes the bug before fixing it.** Confirm it fails on the current code, then fix, then confirm it passes. This is not optional even under time pressure — an unverified fix might not fix anything.

## After the fix

Do a short mini-postmortem in 3-5 bullets:

- What let this ship in the first place (missing test, missing validation, unclear contract, monitoring gap)
- The cheapest guardrail that would prevent the *class* of bug, not just this instance (a validation check, a type constraint, an assertion, an alert)
- Whether this is worth doing now or worth filing as a follow-up — say which, don't leave it ambiguous

## Anti-patterns to avoid

Changing multiple things at once and re-running (you won't know which change mattered). Adding a try/catch that silently swallows the error instead of finding the cause. Declaring victory because the specific symptom went away without confirming *why*.
