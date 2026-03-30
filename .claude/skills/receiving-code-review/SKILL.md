---
name: receiving-code-review
description: Use when receiving code review feedback, responding to PR comments, or acting on reviewer suggestions. Triggers on review comments, PR feedback, "the reviewer said", "address the feedback".
---

# Receiving Code Review

Core principle: **Verify before implementing. Ask before assuming. Technical correctness over social comfort.**

## Response Pattern

For each piece of feedback, follow this sequence:

1. **Read completely** — understand all feedback before acting on any of it
2. **Restate requirements** — confirm your understanding of what's being asked
3. **Check the codebase** — verify the feedback is accurate against actual code
4. **Evaluate technically** — is this suggestion correct? will it break anything?
5. **Respond or implement** — either fix it or raise a reasoned objection

## Prohibited Behaviors

Do NOT say:
- "You're absolutely right!"
- "Great point!"
- "Absolutely, I'll fix that right away!"

Instead: describe the technical fix you're making, or ask a clarifying question. The corrected code demonstrates comprehension — verbal performance does not.

## Handling Ambiguity

If any feedback item is unclear: **STOP — do not implement anything yet.**

Ask specifically what the reviewer means before touching code. Partial understanding produces incorrect implementations, especially when feedback items are interconnected.

## Handling External Suggestions

Before implementing any suggestion, verify:
- It won't break existing functionality
- You understand why the original implementation was done that way
- The reviewer has full context of the constraints

**Push back technically when necessary.** External feedback represents evaluated suggestions, not mandates. Technical correctness supersedes social comfort.

## Implementation Order

1. Blocking/critical issues first
2. Simple fixes next
3. Complex changes last

Test each fix individually. When you were wrong and pushed back incorrectly, state the correction factually — no lengthy apologies.
