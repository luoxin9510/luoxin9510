---
name: finishing-a-development-branch
description: Use when development work on a branch is complete and it's time to merge, push, or clean up. Called by subagent-driven-development and executing-plans after all tasks complete. Triggers on "I'm done", "wrap up", "finish the branch", "ready to merge".
---

# Finishing a Development Branch

Core principle: **Verify tests → Present options → Execute choice → Clean up.**

## Process

### 1. Verify Tests Pass

Run the full test suite. If tests fail, STOP — do not present options until tests are green.

```bash
# Run project tests (adapt to your stack)
npm test / pytest / cargo test / go test ./...
```

### 2. Determine Base Branch

```bash
git log --oneline -10
git branch -r
```

Identify the branch this work diverged from (usually `main` or `develop`).

### 3. Present Options

Show the user exactly these four options:

```
1. Merge back to base branch locally
2. Push and create a Pull Request
3. Keep the branch as-is (do nothing)
4. Discard this work
```

Wait for their choice.

### 4. Execute Choice

**Option 1 — Merge locally:**
```bash
git checkout <base-branch>
git merge <feature-branch>
# Run tests again on merged result
```

**Option 2 — Push and PR:**
```bash
git push -u origin <feature-branch>
# Create PR via gh or mcp__github__ tools
```

**Option 3 — Keep as-is:**
No action needed. Confirm the branch name to the user.

**Option 4 — Discard:**
Require the user to type "discard" explicitly before proceeding.
```bash
git worktree remove <worktree-path>  # if using worktrees
git branch -D <feature-branch>
```

### 5. Clean Up Worktree (Options 1 and 4 only)

```bash
git worktree remove <worktree-path>
```

## Safeguards

- Never merge without verifying tests on the merged result
- Option 4 requires explicit "discard" confirmation — no accidents
- Worktree cleanup only on Options 1 and 4
