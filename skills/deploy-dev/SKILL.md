---
name: deploy-dev
description: Deploy current branch to dev stack and run post-deploy verification (Step Functions + logs + basic checks).
---

# Deploy Dev

## Overview

Use this skill to deploy the current branch to the **dev** environment, then verify deployment health with AWS CLI and workflow/runtime logs.

## When to use

- User asks to deploy to dev
- User asks to test latest changes on dev
- User asks for post-deploy Step Functions/log verification

## Preconditions

- In repo root with clean enough working tree for deployment
- AWS credentials/profile available
- `infrastructure/` dependencies installed

## Workflow

1. Pre-check
   - `git status -sb`
   - confirm branch and latest commit
2. Deploy dev stack
   - run in `infrastructure/`: `npm run deploy`
   - if profile needed: `DEPLOY_PROFILE=<profile> npm run deploy`
3. Post-deploy workflow verification
   - confirm state machine exists: `translator-workflow-dev`
   - inspect latest execution status, duration, and task durations
4. Lambda log verification
   - pull translator lambda latest stream/events
   - summarize request counts, retry/fail count, and key latency signals
5. Report
   - deployment result
   - execution status and timing
   - notable warnings/errors and next actions

## Command snippets

```bash
# deploy
npm run deploy

# if profile override is needed
DEPLOY_PROFILE=translator npm run deploy
```

```bash
# latest executions
aws stepfunctions list-executions \
  --state-machine-arn "arn:aws:states:us-east-1:<account-id>:stateMachine:translator-workflow-dev" \
  --max-results 5
```

```bash
# translator lambda recent logs
aws logs filter-log-events \
  --log-group-name "/aws/lambda/TranslatorLambda-dev" \
  --limit 100
```

## Output format

- Deploy: success/fail + stack highlights
- Workflow: latest execution status + total duration + key task durations
- Logs: retry/failure summary + important error lines
- Recommendation: proceed / rollback / watch more runs

## Safety rules

- Default to **dev only**; do not deploy beta/prod unless explicitly requested
- Do not run destructive git commands
- Do not push unrelated commits during deployment flow
