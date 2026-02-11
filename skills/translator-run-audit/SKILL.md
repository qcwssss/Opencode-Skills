---
name: translator-run-audit
description: Project-only post-translation log audit for serverless-translator. Summarize API batch counts, per-request segment size/latency, failure-retry-split chain, execution time, and effective token/concurrency settings.
---

# Translator Run Audit (serverless-translator)

## Scope

Use this skill only for:
- `/Users/qiuchen/Desktop/Workplace/EpubMaster/serverless-translator`

Do not use for other repositories.

## What this skill produces

For each translation execution:
- Step Functions total duration + key task durations (`SplitEpub`, `TranslateWorkspace`, `AggregateEpub`)
- Total Gemini API request count
- Per-request details: `batch_size`, `attempt`, `split_depth`, `latency_ms`, `outcome`
- Failure chain summary: failed requests, retries, split-to-success path
- Effective runtime knobs: `target_input_tokens`, `concurrency`
- Persisted local audit files under `docs/audit/` (one file per execution)

## Persistence rules (mandatory)

After each audit run, always write Markdown report files to:
- `docs/audit/`

If folder does not exist, create it first.

### File naming convention (mandatory)

Use one file per execution, and include date + total time + file size in filename:

`YYYY-MM-DD__<execution-name>__<total-seconds>s__<source-bytes>B.md`

Example:

`2026-02-10__s3-source-test-1770745084__118.97s__311926B.md`

Notes:
- `YYYY-MM-DD` uses execution start date (local timezone in report body is allowed).
- `total-seconds` uses workflow total duration from Step Functions.
- `source-bytes` uses S3 source object size from `s3://<source-bucket>/<source-key>` via `head-object`.
- If source size cannot be resolved, use `unknownB`.

## Inputs

- Required: execution ARN or execution name
- Optional: compare against baseline execution ARN
- Optional: stage (default `dev`)

## Standard workflow

1. Resolve execution
   - If only name is provided, map to ARN via `list-executions`.
2. Pull Step Functions timing
   - `describe-execution`
   - `get-execution-history` (derive per-task durations)
3. Pull Translator Lambda logs
   - locate request stream via REPORT line in execution time window
   - parse request lines (`batch/done/failed`)
4. Summarize runtime knobs
   - from `Gemini dynamic batching prepared` (preferred)
   - fallback to Lambda env vars if needed
5. Output fixed report format (below)

## Log patterns (must support both formats)

Prefer new format:
- `Gemini dynamic batching prepared: segments=... batches=... target_input_tokens=... concurrency=...`
- `Gemini API request batch: batch_index=... request_no=... batch_size=... attempt=... split_depth=...`
- `Gemini API request done: batch_index=... request_no=... batch_size=... status=ok latency_ms=...`
- `Gemini API request failed: batch_index=... request_no=... batch_size=... attempt=... split_depth=... error=...`

Backward compatibility:
- `Gemini API request batch: request_no=... batch_size=... attempt=... split_depth=...`
- `Gemini API request done: request_no=... batch_size=... status=ok latency_ms=...`
- `Gemini API request failed: request_no=... batch_size=... attempt=... split_depth=... error=...`

## Output format (always)

### 1) Execution summary
- execution name/arn/status
- source key/provider/target/output mode
- source object size (bytes)
- output object size (bytes, if available)
- workflow total seconds
- task seconds (`SplitEpub`, `TranslateWorkspace`, `AggregateEpub`)

### 2) API summary
- total requests / success / failed
- retry request count (`attempt>1`)
- split request count (`split_depth>0`)
- avg/max successful latency
- min/max batch size

### 3) Failure-retry chain
- list failed requests with:
  - request seq, batch size, attempt, error
- show recovery path:
  - retried attempts
  - split depth and final successful child batches

### 4) Per-request timeline
- ordered list (by start time), each line contains:
  - seq, chapter (if available), batch_index, request_no
  - batch_size, attempt, split_depth
  - start/end, latency_ms, outcome, error

### 5) Effective knobs
- `model` (provider/model name used by this run)
- `target_input_tokens`
- `concurrency`
- source of truth used (`prepared log` or `lambda env`)

## Required minimal fields (must appear in every saved report)

- model name
- concurrency value
- input token upper limit (`target_input_tokens`)
- workflow total time
- source file size

## Quality checks

Before finalizing report, verify:
- request counts are internally consistent (`batch == done + failed` for terminal attempts)
- no mixed invocation pollution (filter by Lambda `RequestId`)
- if compare mode is used, both executions use same source key/provider/target when claiming perf delta

## Example invocations

```text
Use translator-run-audit for execution s3-source-test-1770745084
```

```text
Use translator-run-audit for execution A and compare with execution B
```
