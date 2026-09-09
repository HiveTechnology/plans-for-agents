---
type: task
title: "Map the work currently in flight and where it is recorded"
description: "Produce one list of every piece of work underway in this repository, with where it lives today, so it can be moved into _plans/ deliberately."
priority: high
workload: normal
status: open
result: ""
stop_reasons: []
error: false
agent_mode: true
tags: [adoption, discovery]
parent_plan: "../../001-adopt-plans.md"
depends_on: []
steps:
  - "- [ ] List open pull requests and branches ahead of the default branch, with a one-line purpose each"
  - "- [ ] List open issues or tickets assigned to this repository, if a tracker is linked"
  - "- [ ] Search the tree for TODO, FIXME, and HACK markers and group them by subsystem"
  - "- [ ] Read any ROADMAP, TODO, or NOTES file at the repository root"
  - "- [ ] Write the list to work-in-flight.md in this folder: one row per item, columns item, where it is recorded, who owns it, likely kind (inbox, standalone task, or task under a plan)"
  - "- [ ] Note in Progress which items look like they belong to the same outcome, because those are candidate plans"
due: ""
worked: []
date: "260909"
edits: ["260909"]
---

# Map the work currently in flight and where it is recorded

## Context

- **Plan:** [001-adopt-plans](../../001-adopt-plans.md): Adopt a _plans folder
  in this repository
- **Blocks:** decide-where-plans-live (the human needs this list to decide)
- **Depends on:** none

- **Primary files:**
  - `work-in-flight.md` in this folder: the deliverable, created by this task
- **Write scope:** this task folder only. Do not create `_plans/inbox/` items
  yet; the scaffold task does that after the decision.

- **Critical documents:**
  - [_plans reference](../../../README.md): what inbox, standalone task, and
    plan task mean

## Progress

## Stop Report

## Results
