---
type: task
title: "Decide where _plans/ lives: one at the root, or one per package"
description: "A human decision, made with both discovery reports in hand, recorded in the plan so the scaffold can be mechanical."
priority: high
workload: easy
status: open
result: ""
stop_reasons: []
error: false
agent_mode: false
tags: [adoption, decision]
parent_plan: "../../001-adopt-plans.md"
depends_on: [map-work-in-flight, survey-agent-setup]
steps:
  - "- [ ] Read work-in-flight.md and agent-setup.md from the two discovery tasks"
  - "- [ ] Present the human with the two shapes and a recommendation: one _plans/ at the root when one team owns the repository; one per package when packages have separate owners"
  - "- [ ] Ask whether the repository keeps decisions anywhere already, so the plan's Decisions section can link there"
  - "- [ ] Record the decision, the date, and the reason under Decisions in the plan spine"
  - "- [ ] Set status: review and hand to the human"
due: ""
worked: []
date: "260909"
edits: ["260909"]
---

# Decide where _plans/ lives: one at the root, or one per package

## Context

- **Plan:** [001-adopt-plans](../../001-adopt-plans.md): Adopt a _plans folder
  in this repository
- **Blocks:** scaffold-and-wire
- **Depends on:** map-work-in-flight, survey-agent-setup

- **Primary files:**
  - [the plan spine](../../001-adopt-plans.md): Decisions section, where the
    answer is recorded
  - [work-in-flight.md](../map-work-in-flight/work-in-flight.md): the first
    report
  - [agent-setup.md](../survey-agent-setup/agent-setup.md): the second report

- **Critical documents:**
  - [_plans reference](../../../README.md): "one `_plans/` per unit of
    ownership"

This is a human-in-the-loop task. The agent prepares the choice and the
recommendation. The human decides. Do not scaffold anything here.

## Progress

## Stop Report

## Results
