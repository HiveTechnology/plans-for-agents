---
name: phase-transition
description: |
  Clean up and reframe a plan's task graph when moving to a new execution
  phase. Use when the active phase is complete, or when discovery changed the
  route enough that the current phase would mislead the next agent.
---

# Phase transition

Reconcile stale tasks, repair contracts, re-evaluate assumptions, and open the
next execution phase under the same plan.

## Routing check

Before creating a phase, decide which of these you are looking at:

- **A new plan**: the durable outcome or the success criteria changed.
- **A new phase**: the outcome is identical, but the route through it must
  change.
- **A repaired task**: the contract is still valid but was wrong or
  incomplete given what was discovered.
- **A new task**: a new bounded unit of work appeared.
- **A superseded task**: a task no longer fits the route and retires.

### The gradient rule

Outcome drift is a slow gradient, not a clean break. No moment announces
"new plan now". So re-ask the question at every phase close and every large
intake of new tasks: is this still the right plan to be in?

The tie-breaker is the plan's Outcome paragraph. When the work actually being
done makes that paragraph read stale, it is a new plan. Until then, even a
large new set of tasks with a new ordering is a phase. "Borderline, probably
a phase" is normal. Outcomes discovered along the way that do not serve the
current Outcome go to the inbox as future plans, not into this one.

## Steps

### 1. Read the current state

Read the plan spine, the active phase named in `current_phase`, and every
open or active task in the plan's `tasks/`.

### 2. Reconcile stale tasks

No task stays in limbo. For each task in the current phase:

- **Done**: complete, with a verified result.
- **Hold**: blocked on something external.
- **Stopped or failed**: read the stop reasons and decide what they mean.
- **Superseded**: obsolete. Set `status: hold`, `result: superseded`, and
  write the reason in the spine. Never leave an obsolete task `open` or
  `active`.

### 3. Repair valid contracts

If a task remains necessary but its scope, files, or done criteria no longer
match reality, update it in place. Do not create a duplicate.

### 4. Re-evaluate assumptions

Write down which of the phase's assumptions held, which did not, and what new
constraints appeared.

### 5. Check the outcome

- **Changed**: stop. Do not create a phase. Propose an update to the plan
  spine or a new plan.
- **Identical, only the route changed**: continue.

### 6. Close the current phase

Set its `status` to `done` or `superseded`, its `result`, a summary of what
moved, and the reason for the transition.

### 7. Open the next phase

Create `phases/NNN-slug.md` with the next ordinal. Define the new route: the
first waves, live assumptions, and the stop or replan triggers.

### 8. Write contracts for the first wave only

Use `task-create` for tasks that are executable now. Do not pre-create tasks
for later waves whose contracts depend on work not yet done.

### 9. Update the pointer

Set `current_phase` in the plan spine to the new phase. Add the closed phase
to Phase History with its result.

## Rules

- **No stale lensing.** Never open a phase while obsolete tasks are still
  `open` or `active`. They mislead the next agent into obsolete work.
- **Keep phases concrete and short.** Schedule the near waves. Plan to
  transition again when discovery requires it.
- **Preserve history.** Never delete a phase file. Each one records what was
  understood at a point in time.
