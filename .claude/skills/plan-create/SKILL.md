---
name: plan-create
description: |
  Create a plan: a durable outcome container with phases and tasks. Use when
  work needs decomposition before execution: multiple steps, cross-cutting
  concerns, sequencing, or when "just make a task" is not enough. Triggers:
  "plan", "break this down", "I need a plan for", "decompose", "let's plan",
  or when a discussion reveals work too large for a single task.
---

# Plan create

You help the human create a plan: a durable outcome container with an
evolving execution phase that decomposes intent into executable tasks.

The full convention is `_plans/README.md`.

## This skill or task-create

- **task-create**: the work is clear, one unit, can be specified precisely.
- **plan-create**: the work needs decomposition: several tasks, dependencies,
  sequencing, or the scope is not yet understood.

A plan that produces one task should have been a task. A task that keeps
growing should have been a plan. Help the human decide which.

## Flow

### 1. Decide where it lives

`_plans/NNN-slug/`, where `NNN` is the next zero-padded ordinal among the
existing plan folders. The number orders plans. It is not a date, a priority,
or a global id. Dates live in frontmatter.

If the repo keeps more than one `_plans/`, the plan lives with the code it
mostly changes.

Planning but not starting yet: `_plans/inbox/PLAN-slug/`.

### 2. Understand the intent

The human-thinking step. Help the human articulate:

- What are we trying to make true?
- Why now? What triggered this?
- What does "done" look like at the plan level?
- Who owns this? Who is it for?
- What systems does this touch?
- What are the hard constraints: deadlines, dependencies, blockers?

Do not rush to tasks. `## Outcome`, `## Why Now`, and `## Success Criteria`
are where the hard thinking happens. If the human cannot articulate them
clearly, the plan is not ready.

### 3. Identify the first phase

A phase is a bounded execution stage against the durable outcome: the route
believed correct under current assumptions. Ask:

- What is the first coherent stage?
- What assumptions does it rely on?
- Which tasks are in scope for it?
- What must run before what?
- Which tasks can sub-agents run in parallel?
- What discovery or blocker should force a replan?
- What carries forward if the phase ends partial?

Phases are not days. Some last hours, some days. The unit is the coherent
stage. When discovery invalidates the route, close the phase and open the next
one under the same plan. Do not replace the plan.

### 4. Decompose into tasks

For each task:

- What exactly needs to happen?
- Which files and systems are involved?
- What depends on what?
- Can this run autonomously, or does it need human judgment?

Use `task-create` conventions: slug-only folder names, full frontmatter,
relative clickable file paths with line numbers, measurable done criteria.

Write full contracts only for the first wave. Later tasks stay as one-line
entries in the plan's Task Graph until discovery makes them real.

### 5. Establish sequencing

The plan spine holds the durable task graph and the current phase pointer.
The active phase holds the execution graph: dependency waves, sequential
steps, parallelizable work. A plan-manager must be able to read the phase and
know what can be dispatched safely.

### 6. Scaffold

```text
NNN-slug/
  NNN-slug.md                  plan spine, from template/plan.md
  phases/
    001-first-route.md         from template/phase.md
  tasks/
    first-task/
      first-task.md            from ../task-create/template/task.md
    second-task/
      second-task.md
```

Templates: `template/plan.md` and `template/phase.md` beside this file, and
`../task-create/template/task.md` for tasks.

As a plan grows, sections may break out into companion files beside the spine:
`requirements.md`, `background.md`, `stakeholders.md`, `delivery.md`. Start
everything in the spine. Break out when it hurts readability.

## Rules

- Plans are folders. The spine gives structure. Tasks give execution.
- Plan folders and phase files are `NNN-slug`. Task folders are slug only.
- Dates live in frontmatter, never in names.
- A stale route creates a new phase, not a new plan.
- Tasks belong to the plan, not to a phase. A phase references the tasks it is
  attempting.
- Do not create empty plans. Articulate intent before scaffolding.
- Do not create a plan for a single task.
- Plan status lives in the plan's frontmatter. Anything that lists plans is an
  index.
