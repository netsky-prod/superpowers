---
name: requesting-code-review
description: Use when independent judgment is needed for a named high-impact risk, shared boundary, or consequential merge
---

# Requesting Code Review

Dispatch a code reviewer when independent judgment is the cheapest reliable
evidence for a named risk or decision. The reviewer gets precisely crafted
context for evaluation — never your session's history.

**Core principle:** Review where it changes a consequential decision.

## When to Request Review

**Use when retained by preflight:**
- A security, compatibility, data-loss, or public-interface risk benefits from independent judgment
- The brief makes a concrete containment or protocol promise involving untrusted input, filesystem boundaries, secret handling, network replay, or response construction
- A shared or externally visible merge is consequential enough to justify a second set of eyes
- A load-bearing boundary will affect several later tasks

**Optional:**
- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

Do not dispatch a reviewer merely because a task ended, a feature is
"major," or a merge exists. First name the risk or decision. Reviewers
check whether the mechanism belongs in scope before judging whether it is
implemented correctly.

Prefer one focused boundary or final review. Per-task review requires a
different named high-impact risk in each task; repeated review of the same
claim is ceremony, not additional evidence.

## How to Request

**1. Get git SHAs:**
```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. Dispatch code reviewer subagent:**

Dispatch a `general-purpose` subagent, filling the template at [code-reviewer.md](code-reviewer.md)

**Placeholders:**
- `{DESCRIPTION}` - Brief summary of what you built
- `{PLAN_OR_REQUIREMENTS}` - What it should do
- `{BASE_SHA}` - Starting commit
- `{HEAD_SHA}` - Ending commit

**3. Act on feedback:**
- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

## Example

```
[Just completed Task 2: Add verification function]

You: Let me request code review before proceeding.

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
HEAD_SHA=$(git rev-parse HEAD)

[Dispatch code reviewer subagent]
  DESCRIPTION: Added verifyIndex() and repairIndex() with 4 issue types
  PLAN_OR_REQUIREMENTS: Task 2 from docs/superpowers/plans/deployment-plan.md
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661

[Subagent returns]:
  Strengths: Clean architecture, real tests
  Issues:
    Important: Missing progress indicators
    Minor: Magic number (100) for reporting interval
  Assessment: Ready to proceed

You: [Fix progress indicators]
[Continue to Task 3]
```

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "I'll just review the diff myself instead of dispatching a reviewer" | You're the coordinator — reviewing the diff inline burns the context window you need to keep driving the work. Dispatch a reviewer subagent: the diff and the evaluation live in its context, and only the findings come back to you. |
| "The reviewer needs my whole session history to understand the change" | Hand it precisely crafted context, never your session's history. That keeps the reviewer on the work product, not your thought process. |

## Red Flags

**Never:**
- Skip a review retained for a named high-impact risk because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback

**If reviewer wrong:**
- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification

See template at: [code-reviewer.md](code-reviewer.md)
