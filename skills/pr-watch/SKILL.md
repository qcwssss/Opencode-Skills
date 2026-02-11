---
name: pr-watch
description: Shortcut skill. Monitor PR comments only (no code changes).
---

# PR Watch

This is a shortcut wrapper for `pr-review-autopilot` in monitor mode.

## Default behavior

- Mode: `monitor`
- No code edits
- No commit/push
- Output: actionable vs non-actionable comment report

## How to use

Just say:

```text
Use pr-watch on this PR
```

Optional PR target:

```text
Use pr-watch on PR #38
```

If PR number is omitted, infer from current branch.

## Internal mapping

Equivalent to:

```text
Use pr-review-autopilot with MODE=monitor PR=<auto>
```
