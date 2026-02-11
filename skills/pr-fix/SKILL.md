---
name: pr-fix
description: Shortcut skill. Semi-auto PR comment fixing with confirmation before commit/push.
---

# PR Fix

This is a shortcut wrapper for `pr-review-autopilot` in semi-auto mode.

## Default behavior

- Mode: `semi-auto`
- Implements valid review comments
- Runs local verification
- **Always asks for confirmation before commit/push**

## How to use

Just say:

```text
Use pr-fix on this PR
```

Optional PR target:

```text
Use pr-fix on PR #38
```

If PR number is omitted, infer from current branch.

## Internal mapping

Equivalent to:

```text
Use pr-review-autopilot with MODE=semi-auto PR=<auto> REQUIRE_CONFIRM_BEFORE_PUSH=true
```
