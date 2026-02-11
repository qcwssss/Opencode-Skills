# PR Review Autopilot Config (Template)

Use these values in your prompt when invoking the skill.

```text
MODE=monitor
PR=<auto|number>
SOURCE_FILTER=all
POLL_INTERVAL=60
REQUIRE_CONFIRM_BEFORE_PUSH=true
RUN_PAGES_BUILD_WHEN_NEEDED=true
```

Example (monitor):

```text
Use pr-review-autopilot with:
MODE=monitor
PR=38
SOURCE_FILTER=all
```

Example (semi-auto):

```text
Use pr-review-autopilot with:
MODE=semi-auto
PR=38
REQUIRE_CONFIRM_BEFORE_PUSH=true
```
