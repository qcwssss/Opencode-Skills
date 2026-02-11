---
name: doc-sync
description: Use when design decisions, feature changes, bug fixes, or progress updates must be reflected in project documentation or progress logs during development.
---

# Doc Sync

## Overview

Keep documentation aligned with confirmed decisions and code changes. Always apply a minimal update before ending a task.

## Triggers

Use this skill when any of the following occur:
- A design decision is confirmed or finalized
- A feature is added, removed, or behavior changes
- A schema/API/config change is made
- A bug fix or test change lands
- The user asks to update plans, progress, or docs

## Workflow

1. Locate existing documentation targets
   - `PROJECT_CHECKPOINT.md`, `DEV_PLAN.md`, `README.md`
   - `docs/plans/`, `docs/design/`, `docs/` root
   - `task_plan.md`, `progress.md`, `findings.md` if present
2. Select the smallest relevant target set
   - Design decision → design/plan doc
   - Feature change → progress doc + README if user-facing
   - Bug fix/tests → progress doc
3. Apply a minimal update immediately
   - Keep it short and factual
   - Prefer one update over asking for more time

## Minimal Update Template

```
## YYYY-MM-DD
- Change: <what was decided/implemented/fixed>
- Scope: <feature/component/files>
- Status: <done/in progress/blocked>
```

## No-Deferral Rules

Do not skip updates because of time pressure. If the user says “no docs today,” ask for explicit opt-out once; otherwise write the minimal update in the nearest progress file.

## Rationalization Counterexamples

| Excuse | Required Response |
| --- | --- |
| “We’re in a rush” | Apply minimal update now (3 lines) |
| “Can’t spend time on docs” | Ask for explicit opt-out once, then update progress log |
| “I’m about to leave” | Do the smallest update immediately |
| “Not sure where docs live” | Ask one question, then pick the closest progress doc |

## Red Flags

- “I’ll update later”
- “Docs can wait”
- “We can skip doc updates this time”

If any red flag appears, stop and do the minimal update first.
