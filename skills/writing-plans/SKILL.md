---
name: writing-plans
description: Use when implementation must cross contributors or contexts, or its dependencies cannot be executed reliably in one context
---

# Writing Plans

## Overview

Write an implementation plan only when sequencing or handoff earned one
during brainstorming preflight. Include the context, boundaries, and
evidence an executor needs without expanding the product or process.

Ordinary sequential work that one agent can hold and execute in the current
context does not need a plan document.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** If working in an isolated worktree, it should have been created via the `superpowers:using-git-worktrees` skill at execution time.

**Save plans to:** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
- (User preferences for plan location override this default)

## Scope Check

If the retained design covers multiple independent subsystems, plan the
smallest useful slices and their dependencies. Do not create one plan per
subsystem by default. Each planned slice should produce a meaningful
deliverable with evidence appropriate to its claims.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

## Task Right-Sizing

A task is the smallest coherent unit that produces a meaningful deliverable.
It does not automatically earn its own test cycle, commit, or reviewer.
When drawing task boundaries: fold setup,
configuration, scaffolding, and documentation steps into the task whose
deliverable needs them; split only where the work has a real dependency,
ownership, or integration boundary. Each task ends with independently
checkable evidence, which need not be an automated test.

## Task Granularity

Describe meaningful implementation moves, not keystrokes or ceremonial
microsteps. Include a test-first cycle only for executable behavior covered
by the retained TDD scope. Include a review checkpoint only where preflight
named the risk or decision it serves. Group setup, validation, and commit
operations with the deliverable they support.

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** Execute with the lightest workflow retained by the preflight. Use superpowers:subagent-driven-development only when independent task handoff is useful; otherwise execute directly.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

**Source:** [approved chat design, requirements, issue, or spec path that
defines the outcome]

## Global Constraints

[Project-wide requirements — version floors, dependency limits, naming and
copy rules, platform requirements — one line each, with exact values copied
verbatim from the source. Every task's requirements implicitly
include this section.]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Evidence: `[exact existing test, pressure scenario, build, inspection, or check retained by preflight]`

**Interfaces:**
- Consumes: [what this task uses from earlier tasks — exact signatures]
- Produces: [what later tasks rely on — exact function names, parameter
  and return types. A task's implementer sees only their own task; this
  block is how they learn the names and types neighboring tasks use.]

- [ ] **Step 1: Implement the coherent change**

```python
def function(input):
    return expected
```

- [ ] **Step 2: Capture the retained evidence**

Run: `[exact command or inspection procedure]`
Expected: `[observable result that supports the named claim]`

- [ ] **Step 3: Commit the deliverable when a commit boundary is useful**

```bash
git add src/path/file.py
git commit -m "feat: add specific feature"
```

When TDD was retained, expand the evidence step into its RED, GREEN, and
refactor cycle. Do not add that cycle to tasks outside the retained TDD
scope.
````

## No Placeholders

Every step must contain the actual content an engineer needs. These are **plan failures** — never write them:
- "TBD", "TODO", "implement later", "fill in details"
- "Add appropriate error handling" / "add validation" / "handle edge cases"
- "Write tests for the above" (without actual test code)
- "Similar to Task N" (repeat the code — the engineer may be reading tasks out of order)
- Steps that describe what to do without showing how (code blocks required for code steps)
- References to types, functions, or methods not defined in any task

## Self-Review

After writing the complete plan, look at its source with fresh eyes. This is a checklist you run yourself — not a subagent dispatch.

**1. Outcome coverage:** Can you point to a task for each retained requirement and to evidence for each supported claim? List any gaps.

**2. Placeholder scan:** Search your plan for red flags — any of the patterns from the "No Placeholders" section above. Fix them.

**3. Type consistency:** Do the types, method signatures, and property names you used in later tasks match what you defined in earlier tasks? A function called `clearLayers()` in Task 3 but `clearFullLayers()` in Task 7 is a bug.

If you find issues, fix them inline. No need to dispatch a reviewer. If a source requirement has no task, add the smallest task that implements it.

## Execution Handoff

After saving the plan, follow the execution mode already retained during
preflight:

- Use superpowers:subagent-driven-development only when tasks are genuinely
  independent and context isolation helps.
- Use superpowers:executing-plans when the plan will be executed in a
  separate session.
- Execute directly when neither delegation nor session handoff earns its
  overhead.

Ask the user to choose only when the choice changes cost, authority, or an
external side effect and their preference is not already known.
