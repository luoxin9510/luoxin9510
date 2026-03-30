---
name: writing-plans
description: Use when a design has been approved and it's time to create a detailed implementation plan. Called after brainstorming completes. Triggers on "write the plan", "create an implementation plan", "plan this out", after design approval.
---

# Writing Plans

Create comprehensive implementation plans that guide developers through complex features — assuming they lack codebase context and need explicit, actionable steps.

## Plan Structure

### Header (required)

```markdown
# Plan: <Feature Name>

**Goal:** [one sentence]
**Architecture:** [key design decisions]
**Tech stack:** [languages, frameworks, tools]
**Base branch:** [branch to work from]
```

### Tasks

Break work into bite-sized tasks (2–5 minutes each). Each task must:

- Have exact file paths
- Include real code (not pseudocode or placeholders)
- Specify commands with expected outputs
- Follow TDD: write failing test → verify failure → implement → verify pass → commit

**No placeholders. No "similar to Task N". No vague instructions.**

## File Mapping

Before writing tasks:
1. Map all files that will be created or modified
2. Design focused units with single responsibilities
3. Keep related code together
4. Follow existing patterns in the codebase

## Code Standards

- DRY (Don't Repeat Yourself)
- YAGNI (You Aren't Gonna Need It)
- Complete, functional code examples — not templates to fill in
- Consistent types and method signatures across all tasks

## Task Template

```markdown
### Task N: <name>

**Files:** `path/to/file.ts`

**Test (write first):**
\```typescript
it('should do X', () => {
  // exact test code
});
\```

**Run and verify it fails:**
\```bash
npm test -- --testPathPattern=file.test.ts
# Expected: 1 failed
\```

**Implementation:**
\```typescript
// exact implementation code
\```

**Verify passes:**
\```bash
npm test -- --testPathPattern=file.test.ts
# Expected: 1 passed
\```

**Commit:**
\```bash
git add path/to/file.ts
git commit -m "feat: implement X"
\```
```

## Self-Review Checklist

Before presenting the plan:
- [ ] Every requirement from the spec maps to a task
- [ ] No vague language ("handle", "manage", "similar to")
- [ ] No TODO/placeholder comments
- [ ] Types/methods consistent across all tasks
- [ ] Each task is 2–5 minutes of focused work

## Execution Handoff

After the plan is approved, offer two approaches:

1. **Subagent-Driven** (recommended) — fresh subagent per task with two-stage review
2. **Inline Execution** — sequential execution with checkpoints every 3 tasks
