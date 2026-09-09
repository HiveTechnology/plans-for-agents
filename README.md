# Plans for agents

A `_plans` folder that keeps AI coding agents from stepping on each other.

Chat is temporary. The plan is the shared memory. Put the goal, the current
route, the task contracts, and the results in files that every agent can read
and update, and you stop being the operating system between them.

## What is here

- `_plans/`: the folder. An inbox, a place for standalone tasks, and one
  worked plan you can hand to an agent on day one.
- `.claude/skills/`: the five skills that run it: `plan-create`,
  `plan-manage`, `task-create`, `task-run`, `phase-transition`.
- `AGENTS.md`: the rules an agent reads before it touches `_plans/`.
- `_plans/README.md`: the reference. Templates, every frontmatter field,
  lifecycles, stop reasons, the progress protocol.

## Three layers

- **The plan** says what must become true and why. It survives a change of
  route.
- **The phase** is the route being tried right now: which tasks run in
  parallel, which wait, and what discovery would force a replan.
- **The task** is a contract for one agent: scope, files, done criteria,
  validation, and permission to stop.

## Use it

1. Copy `_plans/` and `.claude/skills/` into your repo, or start a new repo
   from this one as a template.
2. Point your agent's instruction file at `AGENTS.md`.
3. Tell the agent: "Manage `_plans/001-adopt-plans/`." That plan walks your
   repo through adopting the folder and ends with one real task run through
   it.

The skills are written in Claude Code's skill format. Any other agent reads
them as plain instructions.

## Read more

The article that explains how this came about and why to use it:
[How I structure `_plans` folders for AI coding agents](https://hive.technology/lab-notes/plans-for-agents/).

Hive Technology, 2026.
