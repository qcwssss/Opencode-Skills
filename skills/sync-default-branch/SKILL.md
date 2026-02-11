---
name: sync-default-branch
description: Switch to main/master safely and pull the latest remote changes.
---

# Sync Default Branch

Use this skill when the user asks to "switch to main/master and pull latest".

## Goal

Safely move to the default branch (`main` first, fallback to `master`) and update local branch from origin.

## Workflow

1. Check working tree status
   - Run `git status --short`.
   - If there are uncommitted changes, do **not** discard anything.
   - Tell the user they must commit/stash first, or explicitly approve carrying changes.

2. Detect target default branch
   - Prefer `main` if it exists.
   - Otherwise use `master`.

3. Switch and update
   - Run:
     - `git checkout <target>`
     - `git pull origin <target>`

4. Report result
   - Show current branch and whether it is up to date.
   - Mention if the pull was fast-forward, no-op, or had conflicts.

## Command Pattern

Use a single sequence once target is known:

```bash
git checkout <target> && git pull origin <target>
```

## Safety Rules

- Never run destructive commands (`reset --hard`, `checkout --`, etc.) unless explicitly requested.
- Never change git config.
- Never force-push from this skill.
