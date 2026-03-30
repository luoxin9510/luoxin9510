---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session. Use when a plan is ready and tasks can be dispatched to fresh subagents. Triggers on plan execution, multi-task implementation, "build this feature".
---

# Subagent-Driven Development

Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration.

## Core Process

For each task in the plan:

1. **Dispatch implementer subagent** — fresh context, no session history pollution
2. **Spec compliance review** — did the subagent implement what the plan asked?
3. **Code quality review** — correctness, edge cases, tests, style
4. Issues found → fix and re-review. Never skip a review stage.

## Model Selection

| Task Type | Model |
|---|---|
| Mechanical implementation of isolated functions | Cheaper/faster model |
| Multi-file integration, moderate complexity | Standard model |
| Architecture decisions, complex design | Most capable model |

## Subagent Instruction Template

```
You are implementing Task N from this plan: [paste plan excerpt]

Context:
- Repository: [path]
- Branch: [branch name]
- Worktree: [worktree path]
- Relevant files: [list]

Task: [exact task description from plan]

Requirements:
- Follow TDD: write failing test first, verify it fails, implement, verify pass
- Only modify files relevant to this task
- Do not change [list of files to leave alone]

Return when done:
- Summary of what was implemented
- List of files changed
- Test results
- Any blockers or questions
```

## Critical Safeguards

- Never skip spec compliance review
- Never skip code quality review
- Never proceed with unresolved issues from review
- Never start work on production branches without explicit user consent
- When subagent reports BLOCKED: reassign to capable model, provide more context, or decompose the task

## Dependencies

Requires: `using-git-worktrees`, `writing-plans`, `requesting-code-review`
Ends with: `finishing-a-development-branch`
