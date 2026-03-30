---
name: skill-creator
description: >
  Create new Claude Code skills (slash commands). Use when the user wants to build a custom
  skill, automate a workflow, create a slash command, or package reusable Claude instructions.
  Triggers on: "create a skill", "make a skill", "new slash command", "build a skill", "/skill-creator".
---

# Skill Creator

Help the user design and create a new Claude Code skill — a reusable, invocable prompt
stored as a SKILL.md file in `~/.claude/skills/<skill-name>/`.

## What is a Skill?

A skill is a Markdown file with YAML frontmatter that Claude loads when the user invokes
`/skill-name` or when the Skill tool is called. It acts as an injected system prompt for
a focused workflow.

**File location:** `~/.claude/skills/<skill-name>/SKILL.md`

**Frontmatter fields:**
- `name` — machine-readable identifier (kebab-case, matches directory name)
- `description` — one or two sentences used by Claude to decide when to trigger the skill.
  Should include example trigger phrases after "Use when:" and "Triggers on:".

## Workflow

Make a todo list and work through each step in order.

### 1. Gather Requirements

Ask the user (use AskUserQuestion) if any of the following are unclear:

- **What should the skill do?** (the core task or workflow)
- **When should it trigger?** (what user phrases invoke it)
- **What is the skill name?** (kebab-case, e.g. `my-workflow`)
- **Does it need external tools?** (bash commands, file writes, GitHub, etc.)
- **What should the output/result look like?**

If the user has already provided enough detail, skip asking and proceed.

### 2. Design the SKILL.md

Write a SKILL.md that follows this template:

```markdown
---
name: <kebab-case-name>
description: >
  <One sentence summary of what it does.> Use when the user wants to <use case>.
  Triggers on: "<phrase 1>", "<phrase 2>", "/<skill-name>".
---

# <Skill Title>

<Short paragraph explaining what this skill does and when to use it.>

## Workflow

Make a todo list for all the tasks in this workflow and work on them one at a time.

### 1. <Step name>

<Instructions for this step.>

### 2. <Step name>

<Instructions for this step.>

...

## Wrap up

<What to do when complete. What summary to give the user.>
```

**Design principles:**
- Be specific and actionable — treat each step as an instruction to Claude
- Include concrete examples where helpful
- Specify what tools to use (Read, Edit, Bash, mcp__github__, etc.)
- Keep the description frontmatter short but include trigger phrases
- Don't over-engineer — match complexity to the task

### 3. Create the Skill File

```bash
mkdir -p ~/.claude/skills/<skill-name>
```

Then write the SKILL.md using the Write tool at:
`/root/.claude/skills/<skill-name>/SKILL.md`

### 4. Validate

Verify the skill file exists and is readable:

```bash
cat ~/.claude/skills/<skill-name>/SKILL.md
```

Confirm:
- Frontmatter is valid YAML (name + description present)
- Steps are clear and actionable
- Trigger phrases in description match what the user would type

### 5. Commit and Push (if in a git repo)

If the user wants the skill saved to their repo (e.g. under `.claude/skills/`), copy it there
and commit. Otherwise the skill lives only in `~/.claude/skills/` locally.

For the current task, commit the new skill to the repo:

```bash
git checkout -b claude/skill-creator-superpowers-V9Q3R 2>/dev/null || git checkout claude/skill-creator-superpowers-V9Q3R
cp -r ~/.claude/skills/<skill-name> .claude/skills/
git add .claude/skills/<skill-name>
git commit -m "Add <skill-name> skill"
git push -u origin claude/skill-creator-superpowers-V9Q3R
```

## Wrap up

Tell the user:
- The skill name and how to invoke it (`/<skill-name>` or via the Skill tool)
- Where the file was saved
- A brief summary of what the skill does
- Any recommended next steps or improvements
