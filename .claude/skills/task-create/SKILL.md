---
name: task-create
description: |
  Create implementation tasks: the concrete engineering work that translates a
  plan into agent-executable contracts. Use when the human wants to create a
  new task, break a plan into tasks, update a task, or turn a discussion into
  actionable work. Triggers: "task", "create a task", "break down",
  "implement", or turning discussion into work items.
---

# Task create

You help the human write precise, agent-executable task contracts that leave
no ambiguity about what is built, where, and how.

A task is a bounded work session for one agent, not an order to finish at any
cost. It must be closable on its own terms, and the assigned agent has stop
authority when literal completion would damage the plan or the system.

Do not paste the generic execution contract into every task. `task-run` holds
the execution policy. The task file holds task-specific context, steps,
progress, stop report, and results.

The full convention, with the field reference, is `_plans/README.md`.

## Flow

### 1. Decide where it lives

- Part of a plan: `_plans/NNN-plan-slug/tasks/slug/`
- Standalone: `_plans/tasks/slug/`
- Not ready to start: `_plans/inbox/TASK-slug/`

### 2. Understand the work

- What needs to be built, changed, or fixed?
- Which plan does it belong to? Read the plan spine and active phase.
- Which systems does it touch?
- Which exact files change?
- What does "done" look like?

### 3. Gather precision

- **Scoped outcome**: what the task accomplishes
- **Local done criteria**: what proves it complete on its own terms
- **Out of scope**: what it must not absorb from the plan or phase
- **Stop conditions**: when the agent stops instead of forcing completion
- **Exact file paths** with line numbers, relative to the task file
- **Reference implementations**: existing code to follow
- **Dependencies**: what must exist before it can start
- **Task-local validation**: how it proves its own work without future tasks
- **Blocks and blocked by**: relationships to other tasks

Push for specificity. "Update the backend" is not a task. "Add the `elevation`
field to `MetricRecord` in `src/schema.ts` at line 42, following the
`pressure` field at line 38" is a task.

### 4. Scaffold

```text
_plans/NNN-plan-slug/tasks/slug/
  slug.md
```

Use `template/task.md` beside this file. Fill every frontmatter field. Write
the report from `template/report.md` when the task reaches review.

## Quality checklist

- [ ] Title is specific and action-oriented
- [ ] File paths are relative, clickable, with line numbers
- [ ] Done criteria are testable
- [ ] Validation is task-local, or downstream validation is split out
- [ ] Dependencies are identified and linked
- [ ] `parent_plan` is set when the task belongs to a plan
- [ ] `agent_mode` is `false` if any ambiguity remains
- [ ] Steps are concrete enough that an agent could execute without asking

## Rules

- One task per folder. Folder name equals file name without `.md`.
- Slug only. No date prefix.
- Status is one of `open`, `active`, `hold`, `review`, `done`.
- `agent_mode: true` means zero ambiguity. If unsure, keep it `false`.
- `error: true` is only for a real build, test, or runtime failure.
- A task never carries future-dependent straggler steps to prove the whole
  plan worked. Integration validation gets its own task or lives in the phase.
- Agents may stop with `status: hold`, `result: stopped`, and explicit
  `stop_reasons` when literal completion would be the wrong move.
