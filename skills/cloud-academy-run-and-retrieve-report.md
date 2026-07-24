---
name: Run and retrieve a report
description: Kick off an asynchronous QA (Cloud Academy) report job, poll it to completion, and retrieve the result.
api: openapi/cloud-academy-openapi-original.json
operations: [v2_reports_user-activity_create, v1_reports_jobs_list, v1_reports_jobs_read]
---

# Run and retrieve a report

QA reporting is asynchronous: you create a report job, then poll the jobs resource for status and data.

## Authenticate

POST `https://platform.qa.com/oauth2/token/` with Basic auth `CLIENT_ID:CLIENT_SECRET` and body `grant_type=client_credentials`; reuse the bearer token (10 hours) on every call.

## Steps

1. **Create a report job** — call a report create operation, e.g. `v2_reports_user-activity_create` (`POST /v2/reports/user-activity/`). Other reports include content-progress, course-progress-export, programs-content, training-plan-content, and members-export. The response returns a job UUID. Prefer the highest available version (v2/v3) — several `v1_reports_*` operations are deprecated.
2. **List jobs** — `v1_reports_jobs_list` (`GET /v1/reports/jobs/`) to see recent jobs and their status.
3. **Poll the job** — `v1_reports_jobs_read` (`GET /v1/reports/jobs/{id}/`) with the UUID until `status` is complete, then read the data. A 404 means the job does not exist or belongs to another user.

## Rules

- Report throttling is strict: many reports allow **1 call per user per hour**, some **1 per user per 10 minutes**. On HTTP 429, read `Retry-After` and wait — do not hammer the create endpoint.
- Poll job status on a sensible interval rather than tight-looping; the jobs list/read endpoints share the 100 req/min public limit.
- Errors are JSON:API `errors[]` (`{ detail, source.pointer, status }`).
