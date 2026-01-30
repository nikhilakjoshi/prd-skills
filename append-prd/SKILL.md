---
name: append-prd
description: Convert requirements into testable tasks appended to a project's plans/prd.json. Use when given a PRD, feature spec, raw ideas, or requirements to break down into tasks.
---

# append-prd

Convert requirements into testable tasks appended to a project's `plans/prd.json`.

## Inputs

- **Requirements**: PRD doc, raw ideas, bullet points, plan -- any form
- **Repo**: the project repo to ground tasks in

## Process

1. Read/parse the provided requirements
2. Explore the repo -- understand architecture, key files, frameworks, existing code patterns
3. Break requirements into small, independently testable tasks
4. Append tasks to `{project}/plans/prd.json`

## Task schema

Each task in `prd.json` follows this structure:

```json
{
  "category": "functional | ui | api | data | auth | performance | ...",
  "description": "Short task description grounded in the actual codebase",
  "steps": [
    "Concrete verification step referencing real files/components",
    "Another step..."
  ],
  "passes": false
}
```

`prd.json` is a top-level JSON array of these objects.

## Rules

- **Explore first**: always read the repo before generating tasks. Reference real files, components, routes, functions in steps.
- **Append, don't overwrite**: read existing `prd.json` first, parse it, then append new tasks to the array.
- **Create if missing**: if `plans/prd.json` or `plans/` dir doesn't exist, create them. Initialize with `[]` before appending.
- **All tasks start unverified**: every task gets `"passes": false`.
- **Small tasks**: each task should be independently testable. Prefer many small tasks over few large ones.
- **Concrete steps**: verification steps must be specific enough that someone (or a test) can confirm pass/fail. Reference actual code paths, components, endpoints.
- **Appropriate categories**: use `functional`, `ui`, `api`, `data`, `auth`, `performance`, or other fitting categories. Match the nature of the task.
- **No duplicates**: if appending to an existing prd.json, check for duplicate/overlapping tasks and skip them.
