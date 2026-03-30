---
name: using-superpowers
description: Use at the start of every session or task to check which superpowers skills apply. This skill governs when and how to invoke other skills. Triggers on any new task, question, or request.
---

# Using Superpowers

Before any response — even clarifying questions — check if a superpowers skill applies.

**If a skill applies to your task, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.**

## Instruction Hierarchy

1. **User's explicit instructions** (highest priority)
2. **Superpowers skills** (override default behavior)
3. **Default system prompt** (lowest priority)

## The 1% Rule

If you think there is even a **1% chance** a skill might apply to what you are doing, you **ABSOLUTELY MUST** invoke the skill.

## Skill Selection Order

When multiple skills could apply, prioritize in this order:

1. **Process skills first** — `brainstorming`, `systematic-debugging`, `test-driven-development`
2. **Planning skills** — `writing-plans`
3. **Execution skills** — `executing-plans`, `subagent-driven-development`
4. **Support skills** — `requesting-code-review`, `using-git-worktrees`, `dispatching-parallel-agents`
5. **Completion skills** — `finishing-a-development-branch`, `verification-before-completion`

Process skills determine the *approach* — always check them first.

## Red Flags (You're Bypassing Skills)

These thoughts mean you should stop and check for applicable skills:

- "This is just a simple question"
- "I need context first before invoking a skill"
- "The skill doesn't quite apply here"
- "I'll use the skill after I understand the problem"
- "This is different from what the skill is for"

**All of these are rationalizations. Check for skills first.**

## Quick Reference

| Situation | Skill |
|---|---|
| User wants to build something | `brainstorming` |
| Plan exists, time to build | `writing-plans` → `executing-plans` |
| Bug or error to fix | `systematic-debugging` |
| Writing any code | `test-driven-development` |
| Tasks are independent | `dispatching-parallel-agents` |
| About to claim work is done | `verification-before-completion` |
| Work is done | `finishing-a-development-branch` |
