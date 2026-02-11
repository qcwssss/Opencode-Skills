---
name: pr-review-autopilot
description: Global PR review assistant with two switchable modes: monitor-only and semi-auto fix loop.
---

# PR Review Autopilot (Global)

Use this skill when you want repeatable PR comment handling in **either**:
- `monitor` mode (observe + report only)
- `semi-auto` mode (observe + evaluate + implement valid items, then wait for your confirm before commit/push)

This skill is intended to work across repositories.

## Mode Switch

Read mode from user instruction first. If not provided, default to `monitor`.

- `MODE=monitor`
- `MODE=semi-auto`

Never run full-auto push loops in this skill.

## Required Inputs

- PR number (or infer from current branch with `gh pr list --head <branch>`)
- Base branch (`master` or `main`)
- Mode (`monitor` or `semi-auto`)

Optional:
- Review source filter (`gemini`, `human`, `all`)
- Poll interval seconds (default 60)

## Common Commands

Fetch comments:

```bash
gh pr view <PR> --comments
gh api repos/<owner>/<repo>/pulls/<PR>/comments
```

Check status:

```bash
gh pr checks <PR>
```

## Comment Evaluation Rules

For each new comment:

1. Classify severity: `high`, `medium`, `low`, `nit`
2. Classify validity:
   - `valid`
   - `context-dependent`
   - `invalid-for-repo`
3. For `valid`, propose concrete patch scope
4. For `invalid-for-repo`, provide concise technical rebuttal

Always prefer reproducible repo behavior over generic suggestions.

## Mode Behavior

### MODE=monitor

Do:
- Pull new comments
- Produce a compact report:
  - actionable items
  - non-actionable items + reason
  - suggested implementation order

Don't:
- Edit files
- Commit
- Push

### MODE=semi-auto

Do:
1. Pull new comments
2. Implement `valid` high/medium items
3. Run verification:
   - `npm run build`
   - `npm test -- --run`
   - `npm run pages:build` when Cloudflare route/runtime touched
4. Present change summary
5. Wait for explicit user confirm before commit/push

Don't:
- Auto-commit without confirm
- Auto-push without confirm

## Safe Commit/Push Protocol (semi-auto)

After user confirm:

```bash
git add -A
git commit -m "fix(review): address actionable PR feedback"
git push
```

Then optionally retrigger review:

```bash
gh pr comment <PR> --body "/gemini review"
```

## Polling Loop (optional)

If user asks continuous monitoring:
- Repeat fetch every N seconds
- Track last processed comment `id`
- Only report/process delta

## Output Format

When reporting to user, always include:

1. `New Comments`: count
2. `Actionable`: bullets with file path + short rationale
3. `Not Actionable`: bullets with reason
4. `Next Step`: what will happen in selected mode

## Stop Conditions

Stop loop when:
- user says stop
- no new comments in 3 consecutive polls (unless user asked persistent monitoring)
- CI is red and requires manual decision
