---
name: task-run
description: |
  Execute an existing `_plans` task as a bounded intelligent work session.
  Use when an agent is assigned a task file or task folder to implement,
  verify, update, stop, or close. Start the task, keep the task file updated
  as durable memory, complete or stop honestly, and report back.
---

# Task run

You execute one task as an intelligent contract. A task is not an order to
finish at any cost. It is a bounded work session with permission to stop when
the contract is wrong, incomplete, blocked, or harmful to the larger plan.

Your job is not to make the task green. Your job is to protect the plan
outcome while attempting the task. The phase needs your intelligence, not
brute-force completion.

## Core rule

Preserve this order:

1. System integrity
2. Plan outcome
3. Active phase
4. Task contract
5. Task steps

If literal completion would damage a higher layer, stop and write a stop
report. A humble stop beats a destructive "done".

Do not shuffle the same cards and hope for a better hand. If repeated attempts
are not changing the underlying constraints, stop. That is signal the phase
needs: more tasks, a missing document, a missing architecture choice, or a
new route.

The task file is your durable memory. Any note you keep for yourself points
at it and never copies it.

## Start

1. Read the task file first.
2. Read only the linked context the task needs.
3. Confirm the contract is coherent:
   - the scoped outcome is clear
   - local done criteria are clear
   - dependencies are present
   - validation is task-local, not downstream plan validation
   - no step needs a future task before this one can close
4. Set frontmatter: `status: active`, append today's `YYMMDD` to `worked`,
   append today's `YYMMDD` to `edits` whenever the file changes.
5. Add a `## Progress` entry with your starting read.

## Execute

Work only inside the contract. Keep the task file updated as you go. It is
the recovery point across compaction, interruption, and handoff.

After each meaningful chunk, update:

- checked steps
- `## Progress`
- commands run
- files changed
- decisions made
- discoveries that affect this task, the phase, or the plan

Task-local validation belongs in the task. Integration validation belongs in
its own task, the phase execution graph, or the plan's success criteria.

## Stop instead of forcing completion

Stop when continuing would create bad work or fake certainty. Use
`status: hold`, `result: stopped` or `failed`, and `stop_reasons`.

Stop especially when:

- you hit the same class of failure twice without learning a new constraint
- a workaround would become architecture
- completing the task requires inventing a missing decision
- the task asks for validation that depends on future work
- the implementation would create an unauthorized shim, fallback, or duplicate
  system
- you are tempted to "just make it pass"

Stop reasons:

- `human_needed`: a decision, credential, scope call, or acceptance criterion
- `agent_needed`: a specialist, reviewer, or sub-agent
- `failure_experienced`: a build, test, runtime, or tool failure
- `architecture_conflict`: executable but appears wrong for the system
- `scope_invalidated`: no longer matches the active plan or phase
- `dependency_missing`: a prerequisite task or artifact is missing

Write `## Stop Report` with: what you were asked to do, why you stopped,
evidence, the recommended next step, and the impact on the active phase.

A stopped task is a successful task if it prevents bad work and gives the
phase better information.

## Before close: work leaves the machine

If you committed anywhere, push before you set `status: review` or `done`.

1. Name the repositories you changed.
2. Push each branch (`git push -u` if it is new).
3. If you cannot push (auth, policy, unknown remote): stop with
   `human_needed`. Never delete, reset, or "clean" local branches to make the
   status look green.

Uncommitted intentional work in progress: commit and push it, or list the
paths in `## Progress` and stop `partial`. Never leave silent machine-only
state.

## Close

A task closes when its own scoped work is complete and task-local validation
is done. Do not hold it open for stragglers that depend on future work.

**Complete:**

1. Pushed, if you committed.
2. `status: review`, unless the task explicitly allows the agent to mark
   `done`.
3. `result: completed`, `error: false`.
4. Every step checked or forwarded.
5. `## Results` filled: summary, verification, files changed, where the work
   was pushed, integration follow-ups.

**Partial but useful:**

1. `status: hold` or `review`, depending on whether the human or the phase
   must act.
2. `result: partial`.
3. Exactly what moved and what remains.

**Failed:**

1. `status: hold`, `result: failed`, `error: true`.
2. `failure_experienced` in `stop_reasons`.
3. The command, its output, the suspected cause, and what you tried.

## Report back

Keep it short:

- task result
- files changed
- validation performed
- stop reason, if stopped
- phase-impacting discoveries or new tasks needed
