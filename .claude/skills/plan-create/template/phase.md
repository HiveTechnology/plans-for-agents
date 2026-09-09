---
type: plan-phase
title: "{PHASE_TITLE}"
description: "The current bounded execution stage against the parent plan."
status: active
result: ""
sequence: 1
parent_plan: "../{PLAN_FILE}.md"
tasks: []
waves:
  - name: "{WAVE_NAME}"
    mode: "parallel"
    depends_on: []
    tasks: []
started: "YYMMDD"
ended: ""
date: "YYMMDD"
edits: ["YYMMDD"]
---

# {PHASE_TITLE}

## Start State

<!-- What is true at the start of this phase: facts, blockers, partial
     completions, discoveries that shape it. -->

## Execution Hypothesis

<!-- Given the current facts, what route moves the plan forward? This is
     allowed to be wrong. The phase exists to test it. -->

## Execution Graph

<!-- Group this phase's tasks into dependency waves. A wave is a set of tasks
     at the same dependency level. parallel: the tasks can be dispatched at
     once, if their write scopes do not overlap. sequential: order inside the
     wave matters. Tasks live under ../tasks/ and may carry across phases. -->

### Wave 1: {WAVE_NAME}

- **Depends on:** none
- **Mode:** parallel | sequential

1. [task-slug](../tasks/task-slug/task-slug.md): why it is in this wave

### Wave 2: {WAVE_NAME}

- **Depends on:** Wave 1
- **Mode:** parallel | sequential

1. [task-slug](../tasks/task-slug/task-slug.md): why it runs after Wave 1

## Stop Or Replan Conditions

<!-- What discovery, blocker, or changed assumption ends this phase and
     triggers the next one? -->

-

## Discoveries

<!-- Append as they happen. Durable decisions go to the plan's Decisions
     section. -->

## Result

<!-- Fill when closing the phase.

Outcome: completed | partial | blocked | invalidated | superseded | abandoned

What moved:
What did not move:
What changed our understanding:
Carry-forward for the next phase:
-->
