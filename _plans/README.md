# `_plans/` reference

The full convention. `../AGENTS.md` is the short law. This file is what the
skills point at for templates, fields, lifecycles, and protocols.

## What `_plans/` holds

Everything actionable. If something needs doing, it goes here.

```text
_plans/
  inbox/                                 raw work, always returns to empty
    map-rendering-glitch/
      map-rendering-glitch.md
      screenshot.png
  tasks/                                 standalone resolved work
    fix-login-redirect/
      fix-login-redirect.md
      error.log
  001-repo-cleanup/                      a plan: one durable outcome
    001-repo-cleanup.md                  the spine
    phases/
      001-baseline-cleanup-path.md       a bounded execution stage
      002-migration-replan.md
    tasks/
      consolidate-ops/
        consolidate-ops.md
        migration-script.sh
      promote-schemas/
        promote-schemas.md
  002-operations-deploy/
    ...
```

Three kinds of thing, each structurally distinct:

- **`inbox/`**: raw work items. Nothing links to an inbox item. They are
  transient by definition.
- **`tasks/`**: standalone work that did not need a plan. The permanent
  record of planless work.
- **Plan folders**: durable outcome containers with a spine, `phases/`, and
  their own `tasks/`.

One `_plans/` per unit of ownership. A small repo keeps one at its root. A
monorepo may keep one per package. There is no central index. Frontmatter is
the graph view. To list open tasks:

```sh
grep -rl 'status: open' --include='*.md' _plans/
```

---

## Plans, phases, and tasks are different jobs

- **Plan = human thinking.** "We need to clean up the repos." Intent,
  dependencies, consequences, success criteria. The plan preserves the durable
  target: what must become true and why.
- **Phase = bounded execution stage.** "Given what we know now, this is the
  route we are trying." A phase progresses the plan. When discovery changes
  the route, close the phase and open the next one. Phases are cheap. One can
  last hours or days. The unit is the coherent stage, not a calendar period.
- **Task = agent contract.** "Move this folder here. Update these three files.
  Run tests. Report back." Ambiguity removed. Execution-ready.

Most of the human's time goes into writing precise tasks. That is not
overhead. By the time a task says exactly which files to touch and what to do
with them, the problem is solved. The agent is the hands.

---

## How to plan

1. **The plan defines the outcome.** What must become true and why.
2. **The phase proposes a traversal.** Dependency waves, parallelizable work,
   stop and replan conditions.
3. **Task contracts test the traversal.** Each task is a bounded intelligent
   work session. The agent tries to produce the scoped outcome, updates the
   task file as it works, and stops honestly when the contract is wrong,
   incomplete, blocked, or harmful.
4. **Results feed the phase.** Completions advance the traversal. Stops,
   failures, discoveries, missing decisions, and missing documents feed back.
5. **The phase adapts or closes.** The plan-manager may add a task, insert a
   wave, split a task, create a decision task, record a decision in the plan,
   or close the phase and open the next one.

The goal is intelligent execution, not brute-force completion. A task is not
an order to finish at any cost. Agents are expected to say "something is not
right" when literal completion would damage the plan or the system.

The hierarchy:

```text
system integrity
plan outcome
active phase
task contract
task steps
```

### Actors and work state

```text
the agent session = who is thinking
_plans            = what work is being moved
```

A plan-manager session uses `plan-manage`. A task-runner session uses
`task-run`. Whatever notes a session keeps for itself are temporary pickup and
scratch. They are not the source of truth.

```text
plan state  -> plan frontmatter and spine
phase state -> phase frontmatter and result
task state  -> task frontmatter, progress, stop report, results
```

One phase may span several plan-manager sessions. A task-runner executes one
task at a time, and the task file is its durable memory.

### Kicking off a plan-manager

```text
You are the plan-manager for this plan.

Read first:
- AGENTS.md
- .claude/skills/plan-manage/SKILL.md
- _plans/README.md

Manage this plan:
- <path to the plan spine>

Your job:
- read the plan spine
- read the active phase from `current_phase`
- inspect the phase execution graph
- inspect task frontmatter for the current ready wave
- repair or create task contracts if needed
- dispatch only ready tasks
- tell each task-runner to read .claude/skills/task-run/SKILL.md
- reconcile task reports into the phase
- close and create phases when the traversal changes

Do not duplicate durable state into chat or session notes. Durable state
lives in the plan, phase, and task files.

Start by reporting:
1. plan outcome
2. active phase
3. current ready wave
4. tasks ready to dispatch
5. tasks blocked or needing repair
6. your recommended next action
```

---

## Naming

### Plans: numbered semantic folders

`NNN-slug/`. The zero-padded ordinal orders plans within this `_plans/`. It is
not a date, a priority, or a global identifier. Frontmatter tracks dates.

```text
_plans/
  PLAN-management-surface-design/      triaged, not started
  001-repo-cleanup/                    first promoted plan
  002-operations-deploy/               second
```

When promoting a `PLAN-slug` inbox item, take the next available `NNN`. Do not
renumber old plans to fill gaps.

### Phases: numbered execution stages

Same pattern inside `phases/`:

```text
_plans/001-device-demo/
  phases/
    001-baseline-demo-path.md
    002-pairing-blocker-replan.md
    003-demo-polish.md
```

The number preserves stage order. The slug names the stage. Dates live in
frontmatter (`date`, `started`, `ended`, `edits`).

Create a new phase whenever the active route would mislead the next agent:
discovery changes the work order, a blocker changes strategy, the attempt goes
partial, or the plan resumes after enough has shifted that it needs a fresh
execution hypothesis.

### Tasks: slug only

Task folders are descriptive kebab-case slugs with no date prefix. A task's
identity is what it does, not when it was born. Dates live in frontmatter.

```text
_plans/001-repo-cleanup/tasks/
  consolidate-ops/
  promote-schemas/
  update-imports/

_plans/tasks/
  fix-login-redirect/
  tls-cert-renewal/
```

A deferred task moves to the next plan with `git mv`, and no stale timestamp
in the folder name tells a lie.

### Inbox items

```text
_plans/inbox/
  map-rendering-glitch/                raw, untriaged
  TASK-fix-tls-cert/                   triaged as a task
  PLAN-management-surface-design/      triaged as a plan
```

No date prefix. Date is frontmatter only.

---

## Inbox lifecycle

```text
Something arrives -> _plans/inbox/slug/

Triage (rename in place):
  -> TASK-slug/  standalone task, waiting for execution
  -> PLAN-slug/  needs decomposition, waiting for planning

Process:
  TASK-slug/ -> move to _plans/tasks/slug/
  PLAN-slug/ -> promote to _plans/NNN-slug/
  or: absorbed into an existing plan's tasks/, inbox item deleted

Inbox always returns to empty.
Plan folders are permanent records.
_plans/tasks/ is the permanent record of planless work.
```

Triage is a `git mv` that adds the prefix and updates `type:` in frontmatter.
Nothing else changes.

Moving out of the inbox is safe because nothing links to an inbox item. When a
plan absorbs one, the plan creates its own tasks as fresh artifacts and the
inbox item is deleted.

A growing `_plans/tasks/` is pressure. Many standalone fixes about the same
subsystem mean a plan wants to crystallize.

---

## The task

### Frontmatter

```yaml
---
type: task
title: "Short action-oriented title"
description: "One or two sentences. Always quoted."
priority: low | medium | high
workload: easy | normal | hard | extreme
status: open
result: ""
stop_reasons: []
error: false
agent_mode: false
tags: [tag1, tag2]
parent_plan: ""
depends_on: []
steps:
  - "- [ ] Step 1"
  - "- [ ] Step 2"
due: ""
worked: []
date: "YYMMDD"
edits: ["YYMMDD"]
---
```

| Field | Required | Meaning |
|---|---|---|
| `type` | yes | Always `task` |
| `title` | yes | Specific and action-oriented. "Add TLS cert rotation", not "TLS" |
| `description` | yes | One or two sentences. What and why |
| `priority` | yes | `low`, `medium`, `high` |
| `workload` | yes | `easy` (under an hour), `normal` (1 to 4 hours), `hard` (4 to 8), `extreme` (multi-day) |
| `status` | yes | `open`, `active`, `hold`, `review`, `done` |
| `result` | no | Set on close or stop: `completed`, `partial`, `blocked`, `failed`, `stopped`, `superseded`, `abandoned` |
| `stop_reasons` | no | Why the agent stopped. See stop authority. Empty unless `status: hold` |
| `error` | no | `true` only for a real build, test, or runtime failure |
| `agent_mode` | no | `true` means zero ambiguity remains and the agent may execute solo |
| `tags` | no | Free vocabulary |
| `parent_plan` | no | Relative path to the plan spine. Empty for standalone tasks |
| `depends_on` | no | Task slugs this task blocks on |
| `steps` | yes | The execution contract, as quoted checkbox strings |
| `due` | no | `YYMMDD` if there is a deadline |
| `worked` | no | Append-only dates work happened |
| `date` | yes | `YYMMDD` created |
| `edits` | yes | `[YYMMDD]` meaningful modifications |

### Step syntax

| Syntax | Meaning |
|---|---|
| `[ ]` | not started |
| `[/]` | in progress |
| `[x]` | done |
| `[-]` | canceled, with the reason |
| `[>]` | forwarded to another task or plan |
| `[<]` | waiting on a scheduled event |
| `[?]` | question that blocks downstream steps |
| `[!]` | critical, do not skip |
| `[*]` | milestone |

### The task folder

```text
fix-login-redirect/
  fix-login-redirect.md             spine: contract and running progress
  fix-login-redirect-report.md      report: what was accomplished, evidence
  cache/                            agent workspace, gitignored
  screenshot.png                    evidence, committed
  error.log                         evidence, committed
```

**The spine** is the agent's working document: frontmatter (state), context
(the brief), steps (the contract), progress (the running log). The agent reads
it to know what to do and writes progress into it as it works.

**The report** is the human's verification document: what was accomplished,
files modified, testing, known issues, retro notes. The human reads it to
decide "did you actually finish?" It is written when the task reaches review.

Spine for the agent. Report for the human.

### Spine body

```markdown
# {Task title}

## Context

- **Plan:** [plan-slug](../../plan-slug.md): {plan title}
- **Blocks:** {task slug} ({one line})
- **Depends on:** {task slug} ({one line}): {status}

- **Primary files:**
  - [{what}](../../../src/path/file.ts#L42): what to change
  - [{what}](../../../src/path/file.ts#L100-L150): pattern to follow

- **Reference implementation:**
  - [{source}](../../../src/path/reference.ts): what to copy

- **Critical documents:**
  - [{doc}](../../../docs/thing.md)

## Progress
<!-- Running log. Dated entries as work happens. -->

## Stop Report
<!-- Only when the agent stops instead of completing. -->

## Results
<!-- When the task reaches review or done. -->
```

### Status lifecycle

```text
open -> active -> hold (if blocked) -> review -> done
```

- `open`: defined, ready to be picked up
- `active`: someone is working on it
- `hold`: stopped, blocked, or waiting on a decision
- `review`: work done, awaiting verification
- `done`: verified complete

### Closing on its own terms

A task can be marked complete when:

- its scoped work is complete
- task-local verification passed
- integration checks that depend on future work are documented or split into
  their own task
- no remaining step depends on unrelated future work

Do not leave a task open because it carries downstream validation stragglers.
Move those to an integration task, to the plan's success criteria, or to the
phase execution graph. Mark the old step `[>]` if the trail matters.

### Stop authority

A task is not an order to finish at any cost. The agent preserves the plan
outcome and system integrity above the literal wording of the task. If
completing the task literally would make the system worse, the agent stops and
writes a stop report.

`stop_reasons` is a list because causes overlap:

| Reason | Meaning |
|---|---|
| `human_needed` | a decision, credential, scope call, or acceptance criterion is needed |
| `agent_needed` | a specialist, reviewer, or sub-agent is needed |
| `failure_experienced` | a build, test, runtime, or tool failure happened |
| `architecture_conflict` | the task is executable but appears wrong for the system |
| `scope_invalidated` | the task no longer matches the active plan or phase |
| `dependency_missing` | a prerequisite task or artifact is missing |

```yaml
status: hold
result: stopped
stop_reasons: [architecture_conflict, human_needed]
error: false
```

```yaml
status: hold
result: failed
stop_reasons: [failure_experienced]
error: true
```

Stopping for architecture or scope is not bad agent behavior. It is often the
correct result. A humble stop beats "completed" work that damages the plan.

### Task quality checklist

- [ ] Title is specific and action-oriented
- [ ] File paths are relative, clickable, with line numbers
- [ ] Done criteria are testable, not vague
- [ ] Validation is task-local, or downstream validation is split out
- [ ] Dependencies are identified and linked
- [ ] Steps are concrete enough that an agent could execute without asking
- [ ] `agent_mode` is `true` only if zero ambiguity remains

---

## The task progress protocol

Update the task file as you go, not at the end. If the session dies, the task
file is the recovery point.

### While working

- Set `status: active` when you start
- Append today's date to `worked`
- Check off steps as they complete
- If execution differs from the plan, update the step text

### Progress entries

After each completed step or logical chunk:

```markdown
### YYMMDD: {chunk title}
- **What was done:** plain words
- **Commands run:** exact, in order, with the directory they run from
- **Files created or modified:** relative paths
- **Decisions made:** things decided during execution, not in the original spec
- **Gotchas:** what did not work the first time, the fix, and why
- **State after:** what the repo looks like now
```

### Context dump

At session end or on request. The test: a fresh agent reads the task file and
continues without rediscovering anything. Write into `## Progress` and the
report's retro notes:

1. **Decisions with alternatives.** What options existed, what was chosen,
   why, what the rejected options looked like.
2. **Discoveries.** Anything confusing, undocumented, or surprising: paths,
   function names, hidden dependencies, magic values.
3. **Build, deploy, and release quirks.** The error message and the fix.
4. **Commands that matter.** Every one a future agent needs, with its
   directory.
5. **File map.** What was created or modified and what each does.
6. **Tradeoffs accepted.** What shipped that is not ideal, and the condition
   under which to revisit.
7. **Patterns established.** With one concrete example.
8. **What is not done.** Things that look done but are not, stubs, deferrals.
9. **Reproduction.** Exact steps from `git pull` to seeing it work.

The task file is the compressed chat history. A cold reader acts on it.

---

## The plan

### Frontmatter

```yaml
---
type: plan
title: "Plan title"
description: "The durable outcome this plan makes true, and why."
status: active
priority: low | medium | high
owner: ""
customer: ""
target_date: ""
tags: []
sequence: 1
current_phase: "phases/001-first-route.md"
started: "YYMMDD"
depends_on: []
next_plan: ""
date: "YYMMDD"
edits: ["YYMMDD"]
---
```

| Field | Required | Meaning |
|---|---|---|
| `type` | yes | Always `plan` |
| `title` | yes | Short and descriptive |
| `description` | yes | The durable outcome and why |
| `status` | yes | `active`, `done`, `deferred`, `hold` |
| `priority` | yes | `low`, `medium`, `high` |
| `owner` | yes | The human accountable |
| `customer` | no | Who it is for, if anyone outside the team |
| `target_date` | no | `YYMMDD`, empty when open-ended |
| `tags` | no | Free vocabulary |
| `sequence` | yes | The ordinal from the folder name |
| `current_phase` | no | Relative path to the active phase. Empty when none is active |
| `started` | no | `YYMMDD` execution began |
| `depends_on` | no | Plan slugs this plan blocks on |
| `next_plan` | no | The plan that follows |
| `date` | yes | `YYMMDD` created |
| `edits` | yes | `[YYMMDD]` meaningful modifications |

### Body

```markdown
# {Plan title}

## Outcome
<!-- What must become true. -->

## Why Now
<!-- Trigger, pressure, opportunity, or deadline. -->

## Success Criteria
- [ ]

## Current Phase
[001-first-route](phases/001-first-route.md)

## Task Graph
1. [task-a](tasks/task-a/task-a.md): {one line}
2. [task-b](tasks/task-b/task-b.md): {one line}

## Phase History

## Open Questions

## Decisions

## Scope
**In scope:**
**Out of scope:**
```

The Task Graph is durable. The Current Phase is the active route. Phase
History records previous stages and their outcomes. Decisions made during the
plan are recorded under Decisions, or linked from there if the repo keeps a
separate decision record.

### The plan spine is the durable sequencer

The spine owns the task graph and the current phase pointer. Task frontmatter
declares hard dependencies in `depends_on`, but agents read the active phase's
execution graph to know what route is being attempted now and what can run in
parallel.

If the active phase becomes false or misleading, do not keep mutating it until
it forgets what was attempted. Close it with a result and create the next one.

### Companion files

Plans start as one spine. As a plan grows, sections break out into peer files
in the plan folder. The spine keeps a one-line pointer to each.

| Companion | When | Contents |
|---|---|---|
| `requirements.md` | success criteria outgrow a few bullets | requirements, acceptance criteria, out of scope, open questions |
| `background.md` | the "why" has a long history | current state, motivation, trigger, timeline |
| `stakeholders.md` | several organizations or a communication plan | contacts, cadence, escalation, decision authority |
| `delivery.md` | rollout is non-trivial | method, milestones, dependencies, risks |

Small plans never need them. Large ones grow the structure they need.

### Plan sequencing

```yaml
next_plan: 002-operations-deploy
```

Plans are permanent records. `active`, `done`, `deferred`, and `hold` are
frontmatter. The folder ordinal preserves list order. Plans do not move.

---

## The phase

A phase is the current execution hypothesis: the route being tried under
current assumptions. It is not a waterfall milestone and it is not a calendar
day. When the route changes materially, close it with a result and open the
next one.

A phase is also the dispatch map. It must show dependency order and
parallelizable work clearly enough that a plan-manager can dispatch sub-agents
without guessing.

### Frontmatter

```yaml
---
type: plan-phase
title: "Pairing blocker replan"
description: "The current bounded execution stage against the parent plan."
status: active
result: ""
sequence: 2
parent_plan: "../001-device-demo.md"
tasks:
  - "../tasks/fix-pairing-retry/fix-pairing-retry.md"
waves:
  - name: "Unblock the demo path"
    mode: "parallel"
    depends_on: []
    tasks:
      - "../tasks/prepare-demo-account/prepare-demo-account.md"
      - "../tasks/inspect-firmware-state/inspect-firmware-state.md"
  - name: "Pairing path"
    mode: "sequential"
    depends_on: ["Unblock the demo path"]
    tasks:
      - "../tasks/fix-pairing-retry/fix-pairing-retry.md"
      - "../tasks/verify-pairing-on-device/verify-pairing-on-device.md"
started: "YYMMDD"
ended: ""
date: "YYMMDD"
edits: ["YYMMDD"]
---
```

Phase `status`: `active`, `done`, `hold`, `superseded`, `abandoned`.
Phase `result` on close: `completed`, `partial`, `blocked`, `invalidated`,
`superseded`, `abandoned`.

Tasks belong to the plan and may be attempted by more than one phase. A phase
references the subset it is attempting. It does not own them.

### Execution graph

Every active phase carries one. Tasks are grouped into waves:

- A wave is a set of tasks at the same dependency level.
- `parallel` means the tasks in the wave can be dispatched at once.
- `sequential` means order inside the wave matters.
- Later waves name the earlier waves they depend on.
- Hard task-level dependencies still live in task frontmatter `depends_on`.

```markdown
## Execution Graph

### Wave 1: Unblock the demo path
- **Depends on:** none
- **Mode:** parallel
1. [prepare-demo-account](../tasks/prepare-demo-account/prepare-demo-account.md)
2. [inspect-firmware-state](../tasks/inspect-firmware-state/inspect-firmware-state.md)

### Wave 2: Pairing path
- **Depends on:** Wave 1
- **Mode:** sequential
1. [fix-pairing-retry](../tasks/fix-pairing-retry/fix-pairing-retry.md)
2. [verify-pairing-on-device](../tasks/verify-pairing-on-device/verify-pairing-on-device.md)
```

The plan-manager reads the graph to decide what to do directly, what to
dispatch, and what must wait. If the graph changes materially, close the phase
and create the next one.

### Phase transition

A transition is a cleanup and a reframe, not just "create the next phase".
Before moving on: reconcile every task in the phase to a true final state,
repair contracts that are still valid but wrong, write down which assumptions
held and which did not, check that the plan outcome itself is still correct,
close the phase with a result, open the next one, and write contracts only for
its first wave. The `phase-transition` skill walks it.

---

## Everything is a folder

Plans and tasks are folders containing a same-name `.md` spine. Any entry that
might accumulate files gets a folder. A task folder is a self-contained
briefing: the contract plus every piece of evidence.

```text
fix-login-redirect/
  fix-login-redirect.md
  screenshot.png
  error.log
  test-script.sh
```

## Status lives in frontmatter, not location

Plans, phases, and tasks declare status in frontmatter and never move between
folders when it changes. The filesystem is storage. Status is state. Keep them
apart, and links never break.

One exception: a deferred task is reparented to a later plan with `git mv`.
Update its `parent_plan` after the move.

---

## Agent execution model

Context loading order is priority:

```text
1. the task folder      why you exist right now, read first
2. task-run             how to execute and stop cleanly
3. AGENTS.md            how to work in this repo
4. skills and docs      what tools you have
5. source code          where to do the work
6. the task folder      report back here
```

The task folder is entry and exit. The agent's world starts and ends there.
Everything between is supporting context.

Without task structure, an agent needs a human steering it through chat to
disambiguate: the human as the operating system, once per task. With
structured task folders, disambiguation happens before the agent starts, once
per plan. That is what lets several agents run at once with no one steering
five chats.

---

## Two execution postures

### `agent_mode: false` (default): human in the loop

The agent is a co-pilot. It reads the task and summarizes its understanding,
asks clarifying questions before acting, proposes options, executes with
guidance, and sets `status: hold` when it needs input.

While working this way, write down what would have made the task autonomous:
paths discovered, decisions made, edge cases, criteria clarified. That record
makes the next similar task autonomous.

When a task has both a decision and execution, split it: a small human-in-loop
task to decide, a large autonomous task to execute with the decision in hand.

### `agent_mode: true`: autonomous

The agent is an autopilot. It performs the deep review below before touching
anything, verifies the task is fully specified, executes within the contract,
makes implementation decisions without asking, escalates only true
showstoppers, and documents thoroughly.

Setting `agent_mode: true` is the human's promise that all ambiguity is gone.
If the agent finds ambiguity, it sets `status: hold` and stops.

---

## Deep review before autonomous work

### Completeness

- [ ] Requirements are unambiguous and measurable
- [ ] File paths are specified, relative, with line numbers
- [ ] Dependencies are documented
- [ ] Acceptance criteria are testable
- [ ] Edge cases are noted
- [ ] No open questions remain

### Steps

Are the steps sufficient? Should any be broken down? Are verification steps
present? The agent has authority to add, modify, or split steps.

### Ambiguity

Stop if any of these is true:

- "make it better" without metrics
- "update the code" without specific files
- "should work well" without measurable criteria
- several valid approaches and no guidance

On ambiguity: `status: hold`, `error: false`, the specific questions written
into Progress, and no further action.

### Report before executing

1. The task in your own words
2. Completeness assessment
3. Steps analysis: sufficient, or what you added
4. Execution plan: approach, key files, complexity
5. Request to proceed

---

## Error and hold

| Situation | `status` | `error` | `stop_reasons` |
|---|---|---|---|
| Needs a human decision | `hold` | `false` | `[human_needed]` |
| Found ambiguity | `hold` | `false` | `[human_needed]` |
| Architecture conflict | `hold` | `false` | `[architecture_conflict, human_needed]` |
| Scope invalidated | `hold` | `false` | `[scope_invalidated]` |
| Build failure | `hold` | `true` | `[failure_experienced]` |
| Test failure | `hold` | `true` | `[failure_experienced]` |
| Runtime exception | `hold` | `true` | `[failure_experienced]` |

`error: true` means something actually broke. `hold` with `error: false`
means the task stopped without a technical failure.

On an error, document: the message, what you were doing, what you tried, the
root cause if known, a suggested fix. Try to resolve it before escalating.

On a stop without an error, document: what you were asked to do, why literal
completion is wrong or blocked, evidence, the recommended next step, and the
impact on the phase.

### Results

```markdown
## Results

**Outcome:** completed | partial | failed

**Summary:** one to three sentences.

**Key changes:**
- what changed and why

**Files modified:**
- [file.ts:42-60](../../../src/file.ts#L42): what changed

**Testing:**
- [x] case 1: passed
- [ ] case 3: not applicable

**Known issues:** or "None"
```

### Stop report

```markdown
## Stop Report

**Stop reasons:** architecture_conflict, human_needed
**What I was asked to do:**
**Why I stopped:**
**Evidence:**
**Recommended next step:**
**Impact on the current phase:**
```

### Retro notes

```markdown
## Retro Notes

**What went well:**
**Issues encountered:** with resolutions
**Lessons learned:** what a future agent should know
**Autonomy assessment:** (human-in-loop tasks)
- Could this have been autonomous?
- What would make it autonomous?
- Suggested splits:
```

---

## Formatting rules

### YAML

Frontmatter is the single source of truth. Malformed YAML silently breaks
queries.

1. Quote `title` and `description`, always. They grow colons.
2. Quote dates: `date: "260514"`. Unquoted, some parsers make them numbers or
   Date objects.
3. Never omit a field. Use `""` or `[]`.
4. Only the listed status values. Never `completed`, `closed`, or `finished`
   as a status.
5. Steps are quoted checkbox strings.

### Paths

Every path in a plan, phase, task, progress entry, or report is relative to the
file that contains it, and clickable. Never absolute. A hardcoded
`/Users/<name>/...` link is broken for everyone but the machine that wrote it.
Include line numbers for source references:

```markdown
- [main.js:966-1001](../../../src/web/main.js#L966): togglePriorityFilter()
```

### The `cache/` folder

A task folder may carry `cache/` for the agent's scratch: test scripts,
temporary files, backups before a refactor. It is gitignored. Evidence that
matters sits beside the spine, committed.

---

## Autonomy

Default to autonomous. Human-in-loop is a stage, not a destination.

Every human-in-loop task costs constant human attention while autonomous tasks
could be running in parallel. When creating tasks:

1. Ask what would make this task autonomous.
2. Gather aggressively: paths, line numbers, reference implementations, done
   criteria.
3. Split decision from execution.
4. Leave behind enough context that the next similar task is autonomous.

Measure the fraction of tasks that run autonomously. Below about seven in ten,
the tasks are under-specified. The bottleneck is almost never the work. It is
the disambiguation.
