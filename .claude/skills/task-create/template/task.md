---
type: task
title: "{TASK_TITLE}"
description: "One or two sentences. Always quoted."
priority: low | medium | high
workload: easy | normal | hard | extreme
status: open
result: ""
stop_reasons: []
error: false
agent_mode: false
tags: []
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

# {TASK_TITLE}

## Context

- **Plan:** [{PLAN_SLUG}](../../{PLAN_SLUG}.md): {plan title}
- **Blocks:** {task slug} ({one line})
- **Depends on:** {task slug} ({one line}): {status}

- **Primary files:**
  - [{what}](../../../../src/path/file.ts#L42): what to change
  - [{what}](../../../../src/path/file.ts#L100-L150): pattern to follow

- **Reference implementation:**
  - [{source}](../../../../src/path/reference.ts): what to copy

- **Critical documents:**
  - [{doc}](../../../../docs/thing.md)

## Progress
<!-- Running log. Dated entries as work happens.

### YYMMDD: {chunk title}
- **What was done:** plain words
- **Commands run:** exact, in order, with the directory they run from
- **Files created or modified:** relative paths
- **Decisions made:** things decided during execution
- **Gotchas:** what did not work the first time, the fix, and why
- **State after:** what the repo looks like now
-->

## Stop Report
<!-- Only when the agent stops instead of completing. See task-run.

What I was asked to do:
Why I stopped:
Evidence:
Recommended next step:
Impact on the current phase:
-->

## Results
<!-- When the task reaches review or done.

Outcome: completed | partial | blocked | failed | stopped | superseded | abandoned
Summary:
Task-local verification:
Where the work was pushed:
Known integration follow-ups:
-->
