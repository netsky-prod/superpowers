---
name: researching-before-implementation
description: Use when requirements depend on unresolved repository facts, unfamiliar or current APIs, standards, external behavior, security boundaries, or other facts that could change implementation
---

# Researching Before Implementation

## Overview

Resolve implementation-affecting unknowns before writing a spec, plan, or
code. Research removes guesses; it does not decide product preferences or
create a document ceremony.

## Requirement Pass

For every requirement, carry forward one disposition from brainstorming:

| Disposition | Meaning |
|---|---|
| grounded | Existing evidence answers it; cite the path or source |
| research-needed | A discoverable unknown could change implementation |
| human-decision | Preference, trade-off, or authority belongs to the user |
| explicit-assumption | Low-impact, reversible uncertainty is stated openly |

Only `research-needed` items enter the research loop. Every requirement gets
a disposition; not every requirement earns browsing or a probe.

## Research Loop

For each research-needed item:

1. State the exact question and what implementation choice its answer controls.
2. Inspect repository code, configuration, lockfiles, history, and existing
   contracts first.
3. For external or time-sensitive facts, consult current primary sources:
   official documentation, standards, specifications, or original research.
   Do not treat model memory as evidence.
4. If authoritative sources do not establish real behavior, run the smallest
   reversible probe that can. A probe answers the question; it is not a
   throwaway implementation of the product.
5. Record the evidence, conclusion, resulting implementation constraint, and
   any remaining uncertainty.

Corroborate only when the impact or source ambiguity requires it. Do not collect
multiple sources as a quota. Stop when further research cannot change the
implementation decision.

## Compact Output

Keep one row per requirement in chat:

| Requirement | Disposition | Evidence | Implementation constraint | Open issue |
|---|---|---|---|---|

Create a durable research file only when handoff or likely context loss gives
it a concrete job. Link sources and repository paths precisely enough that the
next agent can verify them.

## Exit Gate

Do not move to spec, plan, or code while any item remains `research-needed`.
Convert it only by grounding it, identifying a human decision, or making an
explicit low-impact assumption. If a new unknown appears during implementation,
pause the affected change, resolve that item, update the row, and continue. Do
not restart unaffected work or add a new workflow around the discovery.

## Red Flags

- Recalling current API behavior from memory
- Choosing a library before checking the repository and supported versions
- Asking the user to decide a fact that primary evidence can answer
- Browsing for a product preference only the user can choose
- Writing a research report that changes no implementation constraint
- Treating every bullet as a reason for web research
- Deferring an implementation-changing unknown until coding
