# Working in `_plans/`

This repo carries one convention: actionable work lives in `_plans/` as
files, and agents read and update those files instead of holding state in
chat. Read this file before touching anything under `_plans/`. The full
reference, with templates and every field, is `_plans/README.md`.

## The cardinal rule

State lives in one place: the frontmatter of the plan, phase, or task file.
Everything else references it. A dashboard, an index, a chat message, or a
session note may point at a task. None of them own its status. If an index
says "active" and the task file says `status: done`, the task is done and the
index is wrong.

## Three layers

| Layer | Job | Lives at |
|---|---|---|
| Plan | What must become true, and why. Durable. Survives a change of route. | `_plans/NNN-slug/NNN-slug.md` |
| Phase | The route being tried now: waves, order, stop and replan conditions. | `_plans/NNN-slug/phases/NNN-slug.md` |
| Task | A contract for one agent: scope, files, done criteria, validation, stop authority. | `_plans/NNN-slug/tasks/slug/slug.md` |

Plans are human thinking. Phases are where the plan breathes under discovery.
Tasks are proof the work is ready to execute. When discovery invalidates the
route, close the phase and open the next one under the same plan. Do not
replace the plan.

Two more folders sit beside the plans:

- `_plans/inbox/`: raw work, untriaged. Always returns to empty.
- `_plans/tasks/`: standalone tasks that never needed a plan. Permanent record.

## The loop

1. The plan defines the outcome.
2. The phase proposes a traversal: dependency waves, what can run in parallel,
   what must wait, what would force a replan.
3. Task contracts test the traversal. An agent attempts one, updates the task
   file as it works, and stops honestly when the contract is wrong, blocked,
   or harmful.
4. Results feed the phase. Completions advance it. Stops, failures, and
   discoveries change it.
5. The phase adapts or closes, and the next wave is written.

## Priority order

```text
system integrity
plan outcome
active phase
task contract
task steps
```

Each line is subordinate to the one above it. If finishing a task literally
would damage the plan or the system, the agent stops and writes a stop report.
A humble stop beats a destructive "done".

## Actors

- **plan-manager** runs one plan. It reads the spine and the active phase,
  repairs or creates tasks, dispatches only ready tasks, reconciles reports,
  and replans. Skill: `plan-manage`.
- **task-runner** runs one task. It reads the task file first, works inside
  the contract, keeps the file current, and completes or stops. Skill:
  `task-run`.

An agent's session is not the source of truth. Session notes point at plan,
phase, and task files. They never copy their state.

## Skills

| Skill | Use it when |
|---|---|
| `plan-create` | work needs decomposition: several tasks, dependencies, sequencing, or unclear scope |
| `plan-manage` | running a plan: dispatch, reconcile, replan |
| `task-create` | writing one agent-executable contract |
| `task-run` | executing a task. Every agent assigned a task reads this first |
| `phase-transition` | the route changed enough that the current phase would mislead the next agent |

Skills live in `.claude/skills/<name>/SKILL.md`. They are written for Claude
Code and read as plain instructions for any other agent.

## Rules that do not bend

- Status lives in frontmatter. Files do not move between folders when status
  changes.
- Plans and phases are `NNN-slug`. Tasks are slug only, no date. Every plan
  and task is a folder with a same-name spine file.
- Every path in a plan, phase, or task file is relative to that file and
  clickable. Never absolute.
- Quote `title`, `description`, and dates in YAML. Never omit a field; use
  `""` or `[]`.
- Update the task file as you go, not at the end. If the session dies, the
  task file is the recovery point.
- Dispatch only tasks whose dependencies are satisfied. Parallel dispatch only
  inside a `parallel` wave, and only when write scopes do not overlap.
- Write task contracts for the next wave only. Distant tasks stay sketches
  until discovery makes them real.
- A stopped task with a clear stop report is a good result. A forced "done"
  is not.
