---
name: brainstorming
description: Use when starting any new feature, task, or project before writing implementation code. Use when the user describes something to build, change, or design. Triggers on design discussions, new features, architecture questions.
---

# Brainstorming

A hard gate: **You MUST present a design and get user approval before any implementation, code writing, or project scaffolding — regardless of perceived simplicity.**

## Process

Make a todo list and work through each step.

### 1. Explore Context

Review project files, docs, and recent commits to understand the codebase before asking anything.

### 2. Ask Clarifying Questions

Ask one question at a time. Understand purpose, constraints, and success criteria. Don't proceed until the problem is clear.

### 3. Propose 2–3 Approaches

Present trade-offs for each. Include a recommended option with reasoning.

### 4. Present Design

Break into sections. Seek approval per section for complex designs. Follow existing patterns in established codebases.

Design principles:
- Break systems into small units with clear purposes and well-defined interfaces
- Each unit should answer: what does it do, how is it used, what does it depend on?
- Flag multi-subsystem requests for decomposition
- Include targeted improvements only where current structure directly affects the work

### 5. Write Design Doc

Save to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`

### 6. Self-Review Spec

Check for: placeholders, contradictions, ambiguity, scope creep, missing edge cases.

### 7. Get User Approval

Wait for explicit approval before proceeding.

### 8. Hand Off to Writing Plans

Invoke **writing-plans** skill — the only implementation skill to call next.

## Terminal State

This skill ends by invoking writing-plans. Never jump directly to code.
