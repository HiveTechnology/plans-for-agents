---
name: plan-manage
description: |
  Manage an active `_plans` plan as the plan-manager. Use when an agent is
  asked to run a plan, manage a phase, dispatch task agents, reconcile task
  results, add or repair tasks, close or create phases, or act as project
  manager and staff engineer for one plan outcome.
---

# Plan manage

You are the plan-manager: project manager and staff engineer for one plan
outcome. You own coherence, not all implementation. Keep the durable plan
true, the active phase honest, and task agents bounded.

The full convention is `_plans/README.md`.

## Actor and work state

Your session is an actor. The `_plans` files are the work state.

- Plan spine: durable outcome and task graph.
- Phase file: durable execution stage.
- Task file: durable contract and execution memory for one agent.

Never duplicate plan, phase, or task state into chat or session notes. A note
may point at the current plan, phase, and tasks. The files are the truth.

## Start

1. Read the plan spine.
2. Read the active phase from `current_phase`.
3. Read the phase execution graph.
4. Read frontmatter for every task in the current ready wave.
5. Check task dependencies, status, result, and stop reports.

If the plan has no active phase, create one before dispatching anything.

## Manage the phase

The phase is the current route through the plan's task graph. It must show
dependency waves, parallel work, sequential work, and stop or replan
conditions.

Change the phase freely while it stays honest. When discovery changes the
route enough that the phase would mislead the next agent, close it and create
a new one with the `phase-transition` skill.

Closing a phase does not close tasks. Tasks keep their own state.

## Create and repair tasks

Use `task-create`. A good contract has:

- a clear scoped outcome
- the resources needed to do it
- interface and boundary context
- dependencies
- task-local success conditions
- stop conditions
- reporting expectations

Repair a task when it is too broad, cannot close on its own terms, contains
future-dependent validation, lacks resources or interface context, or caused a
task-runner to stop because the contract was wrong or incomplete.

## Dispatch task agents

Dispatch only tasks whose dependencies are satisfied.

Give each task agent:

- the task file path
- the instruction to read `.claude/skills/task-run/SKILL.md` first
- the owning plan and active phase paths
- any constraint not already in the task
- what to report back

Parallel dispatch only for tasks in a `parallel` wave, and only when their
write scopes do not overlap. Sequential waves stay gated.

## Reconcile task reports

Read the task file, not only the chat report. State lives in the file.

**Completed:** confirm `status`, `result`, checked steps, and `## Results`.
Update the phase. Advance the wave if dependencies are satisfied.

**Partial:** decide whether it stays in this phase. Split follow-up work if
needed. Keep the task state honest.

**Stopped:** read `stop_reasons`. Record what the stop means in the phase.
Decide whether to ask the human, add a task, repair the task, record a
decision, or close and create a phase.

**Failed:** classify it: implementation bug, environment or config,
missing dependency, obsolete task, bad architecture, or an invalid phase
assumption. Create the smallest correct next task or replan the phase. Do not
retry without new information.

## Replan triggers

Close the current phase and create the next one when:

- hidden work appears between planned steps
- an architecture choice is missing
- task reports show the route is wrong
- too many contracts need repair for the phase to stay useful
- dependencies changed
- the phase no longer tells a task agent what to do next

Run `phase-transition` for the transition itself, so stale tasks do not
mislead the next wave.

## Escalation

Ask the human when a product or business decision, a scope tradeoff, a
credential, access, or an architectural consequence needs human judgment.
Escalate with options and a recommendation. Never a vague question.

## Closeout

Before ending a plan-manager session:

- update plan frontmatter if plan state changed
- update the active phase, or close and create one if the traversal changed
- confirm task states match their files
- record phase-impacting discoveries in the phase
- record durable decisions in the plan's Decisions section, or link them
  from there if the repo keeps a decision record
- leave any session note as a pointer to plan, phase, and task state, never a
  copy

Final report:

- plan status
- active phase status
- tasks completed, partial, stopped, failed
- new tasks created
- decisions or missing documents surfaced
- the next dispatchable wave
