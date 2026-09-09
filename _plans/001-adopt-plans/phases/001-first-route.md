---
type: plan-phase
title: "First route: discover, decide, scaffold, prove"
description: "Two discovery tasks in parallel, one decision, one scaffold, one real task as proof."
status: active
result: ""
sequence: 1
parent_plan: "../001-adopt-plans.md"
tasks:
  - "../tasks/map-work-in-flight/map-work-in-flight.md"
  - "../tasks/survey-agent-setup/survey-agent-setup.md"
  - "../tasks/decide-where-plans-live/decide-where-plans-live.md"
  - "../tasks/scaffold-and-wire/scaffold-and-wire.md"
  - "../tasks/run-one-real-task/run-one-real-task.md"
waves:
  - name: "Discover"
    mode: "parallel"
    depends_on: []
    tasks:
      - "../tasks/map-work-in-flight/map-work-in-flight.md"
      - "../tasks/survey-agent-setup/survey-agent-setup.md"
  - name: "Decide"
    mode: "sequential"
    depends_on: ["Discover"]
    tasks:
      - "../tasks/decide-where-plans-live/decide-where-plans-live.md"
  - name: "Scaffold"
    mode: "sequential"
    depends_on: ["Decide"]
    tasks:
      - "../tasks/scaffold-and-wire/scaffold-and-wire.md"
  - name: "Prove"
    mode: "sequential"
    depends_on: ["Scaffold"]
    tasks:
      - "../tasks/run-one-real-task/run-one-real-task.md"
started: ""
ended: ""
date: "260909"
edits: ["260909"]
---

# First route: discover, decide, scaffold, prove

## Start State

The repository has no `_plans/` of its own yet. Work in flight is recorded
wherever it is today: issues, pull requests, a TODO file, someone's head. One
or more coding agents are in use, each reading instructions from its own
file.

## Execution Hypothesis

Two discovery tasks can run at once because they read different things and
write different files. Their reports give the human what they need to make
one decision. The scaffold follows the decision mechanically. One real task
run through the new folder proves the whole loop before anything else is
migrated.

## Execution Graph

### Wave 1: Discover

- **Depends on:** none
- **Mode:** parallel

1. [map-work-in-flight](../tasks/map-work-in-flight/map-work-in-flight.md):
   reads issues, PRs, TODOs. Writes only its own task folder.
2. [survey-agent-setup](../tasks/survey-agent-setup/survey-agent-setup.md):
   reads agent instruction files and tool config. Writes only its own task
   folder.

### Wave 2: Decide

- **Depends on:** Wave 1
- **Mode:** sequential

1. [decide-where-plans-live](../tasks/decide-where-plans-live/decide-where-plans-live.md):
   the human decides, with both reports in hand.

### Wave 3: Scaffold

- **Depends on:** Wave 2
- **Mode:** sequential

1. [scaffold-and-wire](../tasks/scaffold-and-wire/scaffold-and-wire.md):
   mechanical once the decision exists.

### Wave 4: Prove

- **Depends on:** Wave 3
- **Mode:** sequential

1. [run-one-real-task](../tasks/run-one-real-task/run-one-real-task.md): the
   first contract written and run for real.

## Stop Or Replan Conditions

- Discovery finds that work is tracked in a system the team will not leave.
  Then `_plans/` holds contracts only and links out, and the scaffold task
  changes shape. Close this phase and open one that says so.
- The repository turns out to be several independently owned packages with no
  shared owner. Then one `_plans/` per package, and the scaffold task splits.
- The agent in use cannot read a repository instruction file at all. Stop.
  The human decides how instructions reach it.

## Discoveries

## Result
