---
name: requesting-code-review
description: Use when completing a task, feature, or set of changes that should be reviewed before merging. Use after each task in subagent-driven development. Triggers on "review my code", "before I merge", task completion checkpoints.
---

# Requesting Code Review

**Review early, review often.** Dispatch a specialized reviewer with focused context rather than your full session history.

## When Reviews Are Required

- After each task in subagent-driven development
- After completing a major feature
- Before merging to main
- When blocked on a problem
- After resolving a complex bug
- Before large refactors

## Process

### 1. Get Change SHAs

```bash
git log --oneline -10
git diff <base-branch>...HEAD --stat
```

Note the commit SHAs covering your changes.

### 2. Dispatch Code Reviewer Subagent

Provide the reviewer with:
- Commit SHAs or diff range
- The plan/spec being implemented (what it's supposed to do)
- Specific concerns to focus on (if any)
- Files changed and why

Template:
```
Review the changes in commits [SHA1..SHA2].

Context: We are implementing [description from plan].

Files changed:
- [file]: [why it changed]

Please check:
1. Spec compliance — does this match the plan?
2. Code quality — correctness, edge cases, style
3. Tests — adequate coverage?

Return: list of issues by severity (critical/important/minor)
```

### 3. Act on Feedback

- **Critical issues:** Fix before proceeding — no exceptions
- **Important issues:** Fix before merging
- **Minor issues:** Note for later, continue

## Rules

- Never skip reviews because changes seem "simple"
- Never ignore critical issues
- Never proceed with unresolved important problems
- Technically sound pushback is acceptable with justification
