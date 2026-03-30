---
name: dispatching-parallel-agents
description: Use when multiple independent failures or tasks exist across different systems, files, or domains. Use when fixing one problem won't affect another. Triggers on multiple unrelated test failures, independent bug reports, parallel workstreams.
---

# Dispatching Parallel Agents

Dispatch one agent per independent problem domain. Let them work concurrently.

## When to Use

Use parallel dispatch when you have:
- Multiple failures with different root causes
- Independent problem domains (fixing one won't resolve others)
- Tasks that don't share state or dependencies

If failures are dependent or share state → work sequentially instead.

## Agent Dispatch Pattern

Each agent needs:

1. **Narrow scope** — One test file, one subsystem, one clear problem
2. **Self-contained context** — All information needed to solve the problem inline
3. **Clear constraints** — Explicit boundaries on what must NOT be modified
4. **Specific output format** — What to return (root cause summary + changes made)

**Bad:** "Fix all the tests."
**Good:** "Fix agent-tool-abort.test.ts — the test fails because X, don't modify Y."

## Template

```
Task: [specific problem]
File(s): [exact paths]
Root cause context: [what you know so far]
Constraints: [what NOT to change]
Return: summary of root cause + list of changes made
```

## Integration

After all agents complete, integrate results:
- Review each agent's output
- Check for conflicts between changes
- Run full test suite to verify no regressions
- Commit combined changes
