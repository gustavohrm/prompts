---
name: test-driven-development
description: Use when implementing features, bug fixes, refactors, or behavior changes that should follow strict test-first TDD with red-green-refactor.
metadata:
  short-description: Enforce test-first development
---

# Test-Driven Development

## Overview

Use strict test-first development: write a failing test, make it pass with the
smallest possible implementation, then refactor while keeping tests green.

**Core principle:** If the test did not fail first for the expected reason, the
test does not prove the behavior.

**Rule clarity:** Violating the letter of TDD is violating the spirit of TDD.

**Announce at start:** "I'm using the test-driven-development skill to
implement this change."

## Tool Compatibility

- Keep instructions tool-agnostic and avoid provider-specific wording.
- When behavior differs across tools, resolve conflicts in this order:
  OpenCode > Claude Code > Codex CLI > Gemini CLI.

## When To Use

Apply this workflow for:

- new features
- bug fixes
- behavior changes
- refactors that can affect behavior

Possible exceptions (only with explicit user confirmation in this conversation):

- throwaway prototypes
- generated code
- strictly non-executable configuration edits (comments, whitespace, key
  ordering, or descriptive metadata that no runtime/build/deploy tooling reads)

Any configuration change that can affect runtime behavior (flags, permissions,
routing, dependency resolution, build output, or environment loading) requires
tests.

Do not self-approve exception paths. If explicit user confirmation is missing,
default to full TDD.

If there is any uncertainty about runtime impact, treat the change as behavior
changing and run tests.

Exception authorization must be explicit for this exact change in the
conversation. Generic urgency language is not approval to skip TDD.

Config-only exception checklist (all must be true):

- change is limited to comments, formatting, key ordering, or descriptive text
- no runtime/build/deploy-consumed key or value changed
- no flags, permissions, routes, dependency pins, or environment keys changed
- the diff itself clearly proves the above

If any checklist item is false or uncertain, do not use the exception path.

## The Iron Law

```text
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

If production code is written before a failing test:

1. Delete that code.
2. Write the failing test.
3. Re-implement from the test.

No exceptions:

- Do not keep the old code as "reference".
- Do not adapt pre-written code while pretending to do RED.
- Delete means delete.

## Red-Green-Refactor Loop

### 1) RED - Write a Failing Test

Write one small test for one behavior.

Requirements:

- one behavior per test
- clear, behavior-focused test name
- test real behavior, not mock wiring
- avoid mocks unless isolation is truly required

### 2) Verify RED - Watch It Fail

**MANDATORY. Never skip this step.**

Run the targeted test.

Confirm:

- test fails (not errors)
- failure reason matches the missing behavior
- failure is not caused by setup, typos, or stale fixtures

If the test errors, fix setup and re-run until it fails for the expected
missing behavior.

If the test passes immediately, it is not proving new behavior. Fix the test or
choose a different scenario.

### 3) GREEN - Write Minimal Code

Implement the smallest change that makes the failing test pass.

Rules:

- no speculative abstractions
- no unrelated refactors
- no extra features beyond the current test

### 4) Verify GREEN - Watch It Pass

**MANDATORY. Never skip this step.**

Run:

- the targeted test
- nearby related tests
- full relevant suite before finishing
- if baseline tests are flaky, run the exact verification commands before coding
  and capture failing test IDs plus error signatures as baseline evidence

For this skill, "full relevant suite" means all tests in changed modules or
packages, plus tests covering modified interfaces and integration boundaries.
If uncertain, run the broader suite.

Confirm:

- new test passes
- existing tests remain green, or only baseline failures with matching test IDs
  and error signatures remain
- no new warnings or runtime errors

If other tests fail, fix them before proceeding.

Use identical verification commands when comparing post-change results against
baseline evidence.

Do not re-label new failures as "known flaky".

If a flaky test was touched by your change, stabilize it now or quarantine it
with a documented follow-up issue before completion.

### 5) REFACTOR - Improve Design Safely

Refactor only while all tests are green.

Allowed work:

- remove duplication
- improve naming
- extract helpers
- simplify design

Do not change behavior during refactor. If behavior changes, start another RED
cycle first.

### 6) Repeat

Move to the next behavior with a new failing test.

## Test Quality Rules

| Quality | Good | Bad |
| --- | --- | --- |
| Focus | One behavior per test | Multiple behaviors in one test |
| Naming | Describes expected behavior | Vague names like `test1` |
| Intent | Validates externally visible behavior | Validates private implementation details |
| Dependencies | Real collaborators when practical | Heavy mocking without clear need |

## Common Rationalizations

| Excuse | Reality |
| --- | --- |
| "It is too small to test." | Small code breaks too. The test is cheap insurance. |
| "I will add tests after coding." | Passing tests written after code do not prove they can fail correctly. |
| "I already tested manually." | Manual checks are not repeatable regression protection. |
| "Deleting work is wasteful." | Keeping unverified code creates future bugs and rework. |
| "Tests after are equivalent." | Tests-after ask what code does; tests-first define what code should do. |
| "It is spirit, not ritual." | Skipping fail-first breaks the spirit and invalidates TDD proof. |
| "TDD is dogmatic. I am being pragmatic." | Pragmatic delivery includes repeatable tests that fail first. |

If these appear, stop and return to RED.

## Red Flags

Stop and restart the cycle when you see:

- production code added before a failing test
- tests written after implementation "for coverage"
- tests that pass on first run when adding new behavior
- inability to explain why the test failed
- mock-heavy tests that do not validate real behavior
- phrases like "just this once" or "I already manually tested it"
- phrases like "it is spirit, not ritual" or "I am being pragmatic"

## Bug Fix Pattern

For bugs, start by writing a failing regression test that reproduces the issue.

If deterministic reproduction is not immediately possible, write the smallest
deterministic characterization test (contract, property, or boundary case) that
captures the observed failure before changing production code.

If you cannot produce a failing automated test, stop and ask the user before
shipping a bug fix.

Then run the normal RED-GREEN-REFACTOR cycle. Never ship a bug fix without a
reproduction or characterization test.

## Testing Anti-Patterns

When adding or changing tests, especially with mocks, review
`testing-anti-patterns.md`.

## Verification Checklist

Before marking work complete:

- [ ] Every behavior change has a test
- [ ] Each new test was observed failing first
- [ ] Failures happened for the expected reason
- [ ] Implementation was minimal for each GREEN step
- [ ] All relevant tests are passing
- [ ] Refactors preserved behavior
- [ ] Edge cases and error paths were covered

If any box is unchecked, the workflow is incomplete.

## Bottom Line

```text
Test first. Fail first. Minimal pass. Refactor safely.
No failing test first means no TDD.
```
