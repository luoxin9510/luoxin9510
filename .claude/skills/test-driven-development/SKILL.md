---
name: test-driven-development
description: Use when implementing any feature, function, or bugfix. Use before writing any production code. Triggers on "implement", "add feature", "fix bug", "write code", "build". Do NOT write code first.
---

# Test-Driven Development

**Write the test first. Watch it fail. Write minimal code to pass.**

You must observe the test failing before writing implementation. Skipping this step defeats TDD's purpose entirely.

## The Red-Green-Refactor Cycle

### RED: Write One Failing Test

- Write a minimal test demonstrating the required behavior
- Use a clear, descriptive test name
- Use real code — avoid mocks when possible
- The test must not exist yet in the codebase

### Verify RED

Run the test. Confirm it **fails for the expected reason**.

If it passes immediately → your test is wrong or the feature already exists.
If it fails with a syntax error → fix the test, not the implementation.

### GREEN: Write Minimal Implementation

Write only the simplest code that makes the test pass. No extra features. No refactoring yet.

### Verify GREEN

Run the test. Confirm it passes. Confirm no other tests broke.

### REFACTOR

Clean up the code while keeping tests green. Do not add new behavior during refactor.

## The Iron Law

**NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.**

Code written before tests must be **deleted entirely** — not kept as reference, not "adapted while writing tests." Delete means delete.

## Common Rationalizations (All Invalid)

| Excuse | Reality |
|---|---|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests after pass immediately — they prove nothing. |
| "Already manually tested" | Manual tests don't prevent regression. |
| "Deleting work is wasteful" | Sunk cost fallacy. Delete it. |
| "TDD is dogmatic" | TDD is more pragmatic than guess-and-check. |
| "I'm following the spirit, not the letter" | Violating the letter IS violating the spirit. |

## Red Flags — STOP and Start Over

- You wrote code before a test
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "This is different because..."

**All of these mean: Delete the code. Start the cycle over.**
