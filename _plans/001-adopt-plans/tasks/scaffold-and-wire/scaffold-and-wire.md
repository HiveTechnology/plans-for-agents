---
type: task
title: "Create the _plans folders, move in-flight work in, and point every agent at AGENTS.md"
description: "Mechanical once the decision exists: folders, the inbox items from the discovery list, and one pointer line in each agent instruction file."
priority: high
workload: normal
status: open
result: ""
stop_reasons: []
error: false
agent_mode: true
tags: [adoption, scaffold]
parent_plan: "../../001-adopt-plans.md"
depends_on: [decide-where-plans-live]
steps:
  - "- [ ] Read the decision under Decisions in the plan spine. If it is missing, stop with human_needed"
  - "- [ ] Create _plans/inbox/ and _plans/tasks/ at each decided location, with a .gitkeep in each"
  - "- [ ] For each row in work-in-flight.md, create _plans/inbox/<slug>/<slug>.md with a one-paragraph description and a link to where it is recorded today. Do not triage them yet"
  - "- [ ] For each row in agent-setup.md, add one line to that agent's instruction file pointing at AGENTS.md. Do not rewrite the file"
  - "- [ ] If any agent supports a skills folder, copy or link .claude/skills/ to where it expects them"
  - "- [ ] Add _plans/**/cache/ to .gitignore"
  - "- [ ] Commit on a branch and push it. Record the branch under Results"
due: ""
worked: []
date: "260909"
edits: ["260909"]
---

# Create the _plans folders, move in-flight work in, and point every agent at AGENTS.md

## Context

- **Plan:** [001-adopt-plans](../../001-adopt-plans.md): Adopt a _plans folder
  in this repository
- **Blocks:** run-one-real-task
- **Depends on:** decide-where-plans-live

- **Primary files:**
  - [the plan spine](../../001-adopt-plans.md): Decisions section, the input
  - [work-in-flight.md](../map-work-in-flight/work-in-flight.md): what becomes
    inbox items
  - [agent-setup.md](../survey-agent-setup/agent-setup.md): which files get
    the pointer line
  - [AGENTS.md](../../../../AGENTS.md): what the pointer lines point at

- **Write scope:** the decided `_plans/` locations, the agent instruction
  files named in agent-setup.md, and `.gitignore`. Nothing else.

## Progress

## Stop Report

## Results
