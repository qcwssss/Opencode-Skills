# Global Engineering Rules

## Core Principle
- Default to the smallest safe change that satisfies the current request.
- One PR should solve one problem only.

## Scope Control
- Do not bundle unrelated fixes, refactors, docs, copy updates, or UI polish unless explicitly requested.
- If you notice adjacent issues, report them as follow-ups; do not include them in this PR.
- Keep file touch count as low as possible.
- Prefer additive/local edits over broad rewrites.

## PR Discipline
- PR title and content must map to one clear objective.
- Avoid mixing feature work with cleanup.
- If work naturally splits into multiple concerns, create separate PRs.

## Execution Defaults
- Before editing, restate the exact in-scope outcome in one sentence.
- After editing, self-check: "Did I change anything not required for this objective?"
- If yes, revert extra changes before commit.

## Communication
- If a broader improvement is useful but out of scope, list it under "Follow-up (not included)".
- Ask only when ambiguity materially changes scope or behavior.
