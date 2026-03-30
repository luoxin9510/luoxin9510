---
name: using-git-worktrees
description: Use when starting feature work requiring isolation, before executing implementation plans, or when running parallel subagent tasks. Triggers on "create worktree", "isolated workspace", before subagent-driven-development or executing-plans.
---

# Using Git Worktrees

Create isolated git workspaces for parallel branch work without switching branches in your main checkout.

## Directory Selection

1. Check for existing `.worktrees/` or `worktrees/` directory in the repo
2. Look for worktree preferences in `CLAUDE.md`
3. Ask the user if neither exists

## Critical Safety Step (for project-local worktrees)

Before creating a worktree inside the project directory, verify it's gitignored:

```bash
cat .gitignore | grep worktree
```

If not ignored, add and commit first:

```bash
echo ".worktrees/" >> .gitignore
git add .gitignore
git commit -m "gitignore: ignore worktrees directory"
```

**Why:** Prevents accidentally committing worktree contents to the repository.

## Create the Worktree

```bash
# Create new branch + worktree
git worktree add .worktrees/<branch-name> -b <branch-name>

# Or use existing branch
git worktree add .worktrees/<branch-name> <branch-name>
```

## Setup After Creation

Auto-detect project type and install dependencies:

```bash
# Node.js
cd .worktrees/<branch-name> && npm install

# Python
cd .worktrees/<branch-name> && pip install -e .

# Rust
cd .worktrees/<branch-name> && cargo build

# Go
cd .worktrees/<branch-name> && go mod download
```

Run baseline tests to confirm clean starting state:
```bash
# Must pass before any work begins
npm test / pytest / cargo test / go test ./...
```

Report the worktree location and test results to your partner.

## Cleanup

```bash
git worktree remove .worktrees/<branch-name>
git branch -d <branch-name>  # if no longer needed
```

## Safeguards

- Never skip .gitignore verification for local worktrees
- Never assume directory locations without checking
- Never proceed when baseline tests fail without explicit user permission
