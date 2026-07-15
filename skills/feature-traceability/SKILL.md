---
description: Trace and validate a feature's path from spec through implementation across branches, releases, and time - find what shipped where, what's missing, and what's gone stale. Use when the user asks "where did this feature come from," "is this fully rolled out," "audit this feature," "trace this across branches," or wants to confirm a fix/feature is consistently present across release branches.
allowed-tools: Bash(git log *) Bash(git branch *) Bash(git tag *) Bash(git show *) Bash(git grep *) Bash(git diff *) Bash(git describe *)
---

# Feature Traceability

Build the trace from the repo itself — git history is the source of truth for "what actually
shipped where," independent of what a ticket status claims.

## 1. Identify the anchor

Get a concrete handle to search on: a ticket ID, feature-flag name, PR number, or distinctive
keyword. If the user gives something vague ("the export feature"), ask for the anchor or find one
via `git log --all --oneline -i --grep=<term>` and confirm it with the user before trusting it.

## 2. Find the origin

- Locate the spec/ADR if this repo uses spec/ADR conventions (e.g. `docs/specs/`, `docs/adr/` —
  see the `idea-to-spec` and `architecture-decision` skills).
- Find the first commit/PR introducing the anchor: `git log --all --oneline -i --grep=<term>`,
  or `git log --all -S<term>` (pickaxe search) when the term appears in code, not just messages.

## 3. Map implementation across branches and releases

For each relevant commit:

- `git branch --all --contains <sha>` — which branches currently have this commit
- `git tag --contains <sha>` — which released versions include it
- Cross-reference against the branches that matter (e.g. active release/hotfix branches, not
  every stale feature branch) to build a presence matrix

## 4. Flag gaps — this is the actual point of the exercise

- **Backport gaps**: present on `main` but missing from an active release/hotfix branch that
  should have it (customers on that branch don't have the fix/feature).
- **Unintentional regressions**: present on an older branch/tag but absent from `main` or a newer
  release with no revert commit explaining why — investigate whether it was silently dropped
  during a merge/rebase.
- **Flag lifecycle**: if the feature is gated by a flag, is the flag still referenced in code long
  after full rollout (stale flag, cleanup owed), or checked inconsistently (some code paths
  gated, others not — a correctness risk, not just tech debt)?
- **Orphaned work**: implementation exists but nothing reaches it (dead code behind a flag no one
  flips, or a spec/ADR with no corresponding commits — decide which is stale).

## 5. Validate completeness, not just presence

A feature "traced" as present should also have: tests that reference it (so a regression would be
caught), and docs/runbook coverage if it's operationally significant. Note when code exists but
either is missing — that's a gap even if the feature technically "works."

## Output

A table: **Stage | Location | Status** (spec, ADR, implementation PRs/commits, branches
containing it, tags/releases containing it, tests, flag status). Follow it with a short,
risk-ranked list of concrete gaps — name the specific branch/release and the concrete exposure
("release/2.3 is still receiving hotfixes but doesn't have this fix — anyone on that branch hits
the bug"), not a generic "coverage looks incomplete."
