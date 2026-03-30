---
name: verification-before-completion
description: Use before claiming any work is complete, done, finished, or working. Use before saying "it should work", "tests should pass", or "done". Triggers on task completion, "I'm done", "it works", "tests pass".
---

# Verification Before Completion

**NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE.**

Confidence is not evidence. "Should work" is not verified. Agent reports are not independent verification.

## The Five-Step Gate

Before claiming anything is complete or working:

1. **Identify** — what command proves this claim?
2. **Execute** — run the complete command fresh right now
3. **Read** — examine the full output and exit code
4. **Verify** — does the output actually confirm your claim?
5. **Claim** — only now make the claim, and include the evidence

## Prohibited Phrases

Do NOT say any of the following without running verification first:

- "Done!"
- "It should work now"
- "Tests should pass"
- "This should fix it"
- "Looks good"
- "Probably works"
- "The subagent confirmed it works"

## What Counts as Verification

**Valid:**
```bash
$ npm test
✓ 47 tests passed (0 failed)
# Exit code: 0
```

**Not valid:**
- "I'm confident the tests pass"
- "The implementation looks correct"
- "The subagent said it worked"
- Running tests on a subset and claiming the whole suite passes

## Why This Matters

False completion claims break trust. Code that "should work" has shipped with undefined functions that crash in production. Partial verification proves nothing about unverified parts.

**Honesty is a core value. Verify, then claim.**

## Common Shortcuts to Avoid

| Shortcut | Problem |
|---|---|
| Trust agent reports | Agents can be wrong — verify independently |
| Run partial tests | Partial passes don't prove full suite passes |
| Skip verify after small change | Small changes break things |
| "I reviewed the code" | Review ≠ running the code |
