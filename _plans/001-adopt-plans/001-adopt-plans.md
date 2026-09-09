---
type: plan
title: "Adopt a _plans folder in this repository"
description: "This repository's actionable work lives in _plans/ as files that every agent reads and updates, proven by one real task run end to end through the folder."
status: active
priority: high
owner: ""
customer: ""
target_date: ""
tags: [plans, agents, adoption]
sequence: 1
current_phase: "phases/001-first-route.md"
started: ""
depends_on: []
next_plan: ""
date: "260909"
edits: ["260909"]
---

# Adopt a _plans folder in this repository

## Outcome

Work in this repository is coordinated through `_plans/`. Agents read their
contract from a task file, write their progress into it, and stop or close on
its terms. The human reads plan, phase, and task files to know the state of
the work, never a chat transcript.

## Why Now

More than one agent is touching this codebase, or is about to. Without a
shared, durable place for the goal, the route, and the contracts, each agent
carries its own picture of the work in its own context window, and the human
is the only thing keeping them from colliding.

## Success Criteria

- [ ] `_plans/` exists at the agreed place with `inbox/`, `tasks/`, and this
      plan
- [ ] The repository's agent instruction file points at `AGENTS.md`
- [ ] Every piece of work currently in flight is either an inbox item, a
      standalone task, or a task under a plan
- [ ] One real task has been written as a contract, run by an agent with
      `task-run`, and closed with a filled `## Results`
- [ ] The human can answer "what is the state of the work" from files alone

## Current Phase

[001-first-route](phases/001-first-route.md)

## Task Graph

1. [map-work-in-flight](tasks/map-work-in-flight/map-work-in-flight.md): list
   the work already underway and where it is recorded today
2. [survey-agent-setup](tasks/survey-agent-setup/survey-agent-setup.md): find
   which agents are used here and where each one reads its instructions
3. [decide-where-plans-live](tasks/decide-where-plans-live/decide-where-plans-live.md):
   one `_plans/` at the root or one per package, decided by the human
4. [scaffold-and-wire](tasks/scaffold-and-wire/scaffold-and-wire.md): create
   the folders, move the in-flight work in, point the instruction files at
   `AGENTS.md`
5. [run-one-real-task](tasks/run-one-real-task/run-one-real-task.md): write
   one real piece of work as a contract, run it, close it

## Phase History

- [001-first-route](phases/001-first-route.md): active

## Open Questions

- Does this repository already keep decisions somewhere (an ADR folder, a
  wiki)? If so, the plan's Decisions section links there.

## Decisions

## Scope

**In scope:**
- The `_plans/` folder, its first contents, and the wiring that makes agents
  read it
- One real task run through it as proof

**Out of scope:**
- Migrating every historical issue or ticket into `_plans/`. Only work in
  flight moves.
- Changing how the team tracks work outside this repository
