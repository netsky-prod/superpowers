---
name: subagent-driven-development
description: Use when an implementation plan has genuinely independent tasks whose handoff benefits from isolated agent contexts
---

# Subagent-Driven Development

Use fresh agents for independent work without turning delegation into a
mandatory review factory.

**Core principle:** Every dispatch, test, reviewer, and ledger entry must
serve a named deliverable, claim, risk, or coordination need.

## Entry Gate

Use this skill only when brainstorming preflight retained both:

- an implementation plan because sequencing or handoff matters; and
- independent tasks that benefit from isolated contexts.

Execute directly when the work is small, tightly coupled, or cheaper to
hold in one context. The existence of a plan does not itself justify
subagents.

## Execution Preflight

Before the first dispatch, state a compact execution contract:

1. Which tasks are independent enough to delegate.
2. What each task must deliver.
3. What evidence each task must return.
4. Which decisions and interfaces must remain shared.
5. The review budget: none, boundary-only, final-only, or per-task.
6. Whether durable progress tracking is needed.

### Review budget

| Level | Use when |
|---|---|
| None | Reversible work has direct, cheap evidence and no consequential shared boundary |
| Boundary-only | A task creates an interface or decision several later tasks depend on |
| Final-only | Integration risk matters, but task-local independent review would repeat the same judgment |
| Per-task | Each task independently carries a named security, compatibility, data-loss, or irreversible risk |

Per-task review is exceptional. "More eyes" and "the task is complete" are
not risk statements. If you cannot say what decision the reviewer could
change, do not dispatch one.

Use a ledger only when compaction, long execution, or multiple workers make
loss of state plausible. Otherwise normal task tracking is enough.

## Process

### 1. Prepare the workspace

Use an isolated branch or worktree when concurrent work or branch safety
requires it. Do not create a worktree merely because this skill exists.
Never start destructive or externally visible work without the applicable
human gate.

Read the retained plan and correct contradictions before dispatching.
Treat the approved outcome as authority; a plan is a coordination aid, not
a reason to implement unnecessary machinery.

### 2. Dispatch coherent work

Batch small same-shape edits. Split only where tasks can proceed without
writing the same files or depending on unstated intermediate choices.
Never dispatch multiple agents that will race over shared state.

Give each implementer:

- the task-local deliverable and non-goals;
- relevant interfaces and exact constraints;
- the files or artifact paths it must inspect;
- the evidence contract selected in preflight; and
- the report path only when a durable report is actually useful.

Do not paste session history or expose the expected evaluation result.
Templates in this directory are optional aids, not mandatory artifacts.

### 3. Evaluate the result

Inspect the returned diff or artifact and the promised evidence. Do not
require automated tests when the retained deliverable is prose, prompts,
skills, static configuration, or a throwaway probe. Do not accept a test
count as evidence for claims the tests do not cover.

Handle status directly:

- **Done:** integrate it and apply the retained review budget.
- **Needs context:** provide only the missing context and resume.
- **Blocked:** change the task, agent, or assumption; do not repeat the same
  dispatch unchanged.
- **Concern:** decide whether it affects a retained claim or named risk.

### 4. Review only where budgeted

When review is retained, use superpowers:requesting-code-review and give the
reviewer the task or branch diff, the approved outcome, and the named risk.
The reviewer's first question is scope: **should this mechanism exist for
the approved outcome?** Only then review correctness of retained code.

Do not ask a reviewer to re-run evidence already captured unless freshness
or independence is itself the reason for review.

### 5. Resolve findings without ritual loops

For each finding, decide:

- **Required behavior is wrong:** fix it and recheck the covering evidence.
- **Unnecessary machinery is exposed:** remove it; do not harden it.
- **Finding is incorrect or outside scope:** record a short ruling when it
  affects a consequential decision, otherwise move on.
- **New material risk appears:** update the execution contract before more
  work.

One fix and focused recheck is the default. A second round requires new
material information. Repeating equivalent review rounds is not additional
evidence; adjudicate or escalate instead.

### 6. Integrate and finish

After delegated tasks are integrated, run only the final evidence retained
for the product claims. Dispatch a final reviewer only if the review budget
includes one. Then use superpowers:finishing-a-development-branch for the
actual integration choice.

## Stop Conditions

Stop for human direction when work would be destructive or irreversible,
security-sensitive, externally visible beyond the authorized scope, or the
requirements are so contradictory that every path is a guess. Ordinary
implementation choices and reversible corrections do not require a new
ceremony round.

## Red Flags

| Thought | Correction |
|---|---|
| "One subagent per plan task" | Batch by independent judgment and file ownership, not headings. |
| "Every task needs two reviews" | Name the task-specific risk or omit the reviewer. |
| "The reviewer found a bug in our helper" | First ask whether the helper belongs in the product. |
| "Five rounds are safer than one" | Equivalent repeated rounds add no evidence. |
| "We need a ledger because the template has one" | Track durably only when state loss is plausible. |
| "The tests pass, so the prompt package is correct" | Use evidence that exercises the artifact's actual behavior. |

## Platform Notes

Use the platform's native subagent controls. Keep user-owned threads for
user-visible independent work; use subagents for implementation subtasks.
When subagents are unavailable, execute the same retained plan directly
without recreating delegation paperwork.
