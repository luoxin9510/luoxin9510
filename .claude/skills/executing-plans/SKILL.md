---
name: executing-plans
description: Use when a written implementation plan exists and it's time to execute it. Use when the user says "execute the plan", "implement the plan", or "start building". Do NOT use before a plan exists — use brainstorming and writing-plans first.
---

# Executing Plans

Execute written implementation plans through a structured process using subagents.

**REQUIRED:** Use `subagent-driven-development` for execution. Solo execution is lower quality.

## Process

Make a todo list and work through each step.

### 1. Load and Review Plan

Read the plan critically before starting. Raise any concerns or ambiguities with your partner now — not mid-execution.

Check for:
- Unclear steps or missing context
- Contradictions between tasks
- Missing file paths or commands
- Scope that seems wrong

### 2. Set Up Workspace

Use `using-git-worktrees` to create an isolated workspace. Never implement on main/master without explicit user permission.

### 3. Execute Tasks

Work through each task systematically:
- Mark each task in_progress before starting
- Follow plan steps exactly — don't improvise
- Run verifications after each task
- Mark completed only when verified

**STOP executing immediately when:**
- A blocker is hit
- Instructions are unclear
- A verification fails
- The plan seems wrong for the actual codebase

Ask for clarification rather than guessing.

### 4. Complete Development

Use `finishing-a-development-branch` to wrap up.

## Critical Rules

- Follow the plan as written
- No undocumented changes
- Blockers stop execution — escalate immediately
- Tests must pass at each checkpoint
