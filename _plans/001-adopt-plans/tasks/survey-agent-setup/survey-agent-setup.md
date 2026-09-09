---
type: task
title: "Survey which agents are used here and where each reads its instructions"
description: "Find every coding agent configured for this repository and the file each one reads first, so AGENTS.md can be wired into all of them in one step."
priority: high
workload: easy
status: open
result: ""
stop_reasons: []
error: false
agent_mode: true
tags: [adoption, discovery]
parent_plan: "../../001-adopt-plans.md"
depends_on: []
steps:
  - "- [ ] Find agent instruction files at the root and in packages: AGENTS.md, CLAUDE.md, .cursorrules, .github/copilot-instructions.md, and any tool-specific config directory"
  - "- [ ] For each, record which agent reads it and whether it already points at another file"
  - "- [ ] Check whether any agent in use supports a skills folder, and where it expects one"
  - "- [ ] Write agent-setup.md in this folder: one row per agent, columns agent, instruction file, skills location or none, notes"
  - "- [ ] Note in Progress any agent that cannot read a repository file at all, because that is a stop condition for the phase"
due: ""
worked: []
date: "260909"
edits: ["260909"]
---

# Survey which agents are used here and where each reads its instructions

## Context

- **Plan:** [001-adopt-plans](../../001-adopt-plans.md): Adopt a _plans folder
  in this repository
- **Blocks:** decide-where-plans-live, scaffold-and-wire
- **Depends on:** none

- **Primary files:**
  - `agent-setup.md` in this folder: the deliverable, created by this task
- **Write scope:** this task folder only. Do not edit any instruction file
  yet.

- **Critical documents:**
  - [AGENTS.md](../../../../AGENTS.md): what the instruction files will point
    at
  - [the skills](../../../../.claude/skills/): what a skills-capable agent
    will load

## Progress

## Stop Report

## Results
