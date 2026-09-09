---
type: task
title: "Write one real piece of work as a contract, run it, and close it"
description: "Proof that the loop works here: one inbox item becomes a task contract, an agent runs it with task-run, and it closes with Results filled and the work pushed."
priority: high
workload: normal
status: open
result: ""
stop_reasons: []
error: false
agent_mode: false
tags: [adoption, proof]
parent_plan: "../../001-adopt-plans.md"
depends_on: [scaffold-and-wire]
steps:
  - "- [ ] With the human, pick one inbox item that is small, real, and has a testable done condition"
  - "- [ ] Triage it: git mv to TASK-<slug>, then move it to _plans/tasks/<slug>/<slug>.md and write the full contract with task-create"
  - "- [ ] Dispatch an agent to it with the instruction to read .claude/skills/task-run/SKILL.md first"
  - "- [ ] When it reports, read the task file, not the chat. Confirm status, result, checked steps, Results, and that the work was pushed"
  - "- [ ] If it stopped, read the stop report and decide: repair the contract, add a task, or accept the stop as the correct result"
  - "- [ ] Record in Progress what the first run taught about writing contracts in this repository"
due: ""
worked: []
date: "260909"
edits: ["260909"]
---

# Write one real piece of work as a contract, run it, and close it

## Context

- **Plan:** [001-adopt-plans](../../001-adopt-plans.md): Adopt a _plans folder
  in this repository
- **Blocks:** the plan's last success criterion
- **Depends on:** scaffold-and-wire

- **Primary files:**
  - the chosen inbox item, then its task folder under `_plans/tasks/`
- **Critical documents:**
  - [task-create](../../../../.claude/skills/task-create/SKILL.md): how the
    contract is written
  - [task-run](../../../../.claude/skills/task-run/SKILL.md): what the
    dispatched agent reads first
  - [_plans reference](../../../README.md): the stop reasons and the close
    rules you are checking against

The point of this task is not the piece of work. It is the first contract,
the first dispatch, and the first read of a task file instead of a chat.

## Progress

## Stop Report

## Results
