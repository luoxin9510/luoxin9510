---
name: systematic-debugging
description: Use when investigating any bug, error, or unexpected behavior. Use before attempting any fix. Use especially under time pressure when quick fixes seem tempting. Triggers on errors, test failures, unexpected behavior, "why is this broken".
---

# Systematic Debugging

**NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

Quick patches that address symptoms waste time and create new bugs. This process saves time compared to guess-and-check thrashing.

## Four-Phase Framework

### Phase 1: Root Cause Investigation

- Read the full error message — don't skim
- Reproduce the issue consistently before doing anything else
- Check recent changes (git log, git diff)
- Gather diagnostic evidence across all system components
- Trace data flow backward from the symptom to the source

Do not form hypotheses yet. Gather evidence first.

### Phase 2: Pattern Analysis

- Find working examples of similar functionality
- Read reference implementations completely
- Identify exact differences between working and broken code
- Understand all dependencies involved

### Phase 3: Hypothesis and Testing

- Form one specific theory based on evidence
- Test with the minimal possible change
- Verify the result
- If wrong: form a new hypothesis based on new evidence

One hypothesis at a time. No shotgun changes.

### Phase 4: Implementation

1. Write a failing test that captures the bug
2. Implement a single fix addressing the root cause
3. Verify the test passes
4. Verify no regressions

## Critical Safeguard

**If 3 or more fix attempts fail: STOP.**

Do not continue patching. This signals a structural problem. Step back, question the underlying architecture, and discuss with your partner before continuing.

## Red Flags (You're Doing It Wrong)

- Making multiple changes at once
- "Let me just try this and see"
- Fixing before reproducing consistently
- Not reading the full error message
- Skipping the failing test
