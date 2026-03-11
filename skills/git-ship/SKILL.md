---
name: git-ship
description: Commit, push, and create/update PR in one flow.
---

# Git Ship

Use this skill when the user says "git ship" or asks to publish current branch changes quickly.

## Default Operating Mode

- Auto-ship by default after implementation is complete.
- Do **not** ask a separate "git ship?" confirmation when all of these are true:
  1) the user already asked for implementation/fixes,
  2) local verification passed,
  3) branch is not `main`/`master`.
- Only ask before shipping when there is risk/ambiguity (destructive action, secrets, unclear scope, or explicit user preference to review first).

## Goal

Run a safe one-pass release flow:
1) commit local changes,
2) push branch,
3) create a PR (or update/report existing PR).

## Workflow

1. Inspect git state
   - Run `git status --short`
   - Run `git diff --staged; git diff`
   - Run `git log --oneline -8`

2. Stage and commit
   - Stage relevant files.
   - Create one concise commit message matching repo style.
   - Never include obvious secret files.

3. Push branch
   - If no upstream branch, run:
     - `git push --set-upstream origin <current-branch>`
   - Else run:
     - `git push`

4. Create or reuse PR
   - Try `gh pr create` with clear title/body.
   - If PR already exists, report existing PR URL instead of failing.

5. Report
   - Include commit hash, branch, and PR URL.

## PR Body Template

```md
## Summary
- <1-3 bullets>

## Files
- <key paths>

## Verification
- <commands/tests run>
```

## Safety Rules

- Never force push.
- Never amend unless explicitly requested.
- Never skip hooks.
- Do not push directly to `main`/`master` unless user explicitly asks.
