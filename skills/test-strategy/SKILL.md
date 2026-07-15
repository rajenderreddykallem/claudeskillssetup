---
description: Design and write a unit test strategy or suite for new or existing code — what to test, what to skip, mocking boundaries, edge cases. Use when the user asks to add tests, improve coverage, review test quality, or "how should I test this."
---

# Test Strategy

Coverage percentage is a signal, not a target. The goal is tests that catch real regressions and stay stable as implementation details change underneath them.

## What to test, in priority order

1. Business logic and branching (the actual decisions the code makes)
2. Edge cases at the boundaries of valid input (empty, zero, max, off-by-one, unicode/encoding where relevant)
3. Error paths — what happens on invalid input, timeout, dependency failure, not just the happy path
4. Integration seams — where this code's contract with another module/service actually matters

Skip or deprioritize: trivial getters/setters, framework/library glue that has its own test suite, private implementation details that could change without changing behavior.

## Design rules

- **Test behavior/contract, not implementation.** A test that breaks when you rename a private variable but the public behavior is unchanged is a liability, not a safety net.
- **Table-driven / parametrized tests** for combinatorial input spaces — one test function, many cases, instead of copy-pasted near-duplicates.
- **Mock only at architectural boundaries**: network calls, database, filesystem, wall-clock time, randomness. Don't mock code you own internally just to isolate a unit — that couples the test to internal structure and makes refactors break tests that should have kept passing.
- **Name tests by behavior**: `returns_404_when_user_not_found`, not `test_getUser_2`. The test name should tell you what broke without reading the body.
- **Arrange/Act/Assert** (or Given/When/Then) structure, one logical assertion focus per test.
- **Every bug fix gets a regression test** that fails on the old code and passes on the fix. Write it before the fix if possible, to confirm it actually catches the bug.

## Flaky-test smells to flag or fix

Real `sleep()` instead of proper waits/mocked clocks, shared mutable state between tests, unseeded randomness, dependence on wall-clock time, dependence on test execution order, network calls to real external services in a "unit" test.

## Output

When asked to review existing tests, report gaps as: what scenario is untested, and what bug it would currently let through — not just a coverage percentage. When writing new tests, state which category (above) each test targets so gaps are visible by inspection.
