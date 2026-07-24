---
name: Browse the QA content catalog
description: List and inspect QA (Cloud Academy) learning content — courses, learning paths, quizzes, exams, labs — via the content catalog operations.
api: openapi/cloud-academy-openapi-original.json
operations: [v2_content_catalog_list, v2_content_get_entity]
---

# Browse the QA content catalog

Use the QA REST API (base `https://platform.qa.com/restapi`) to browse the learning content catalog.

## Authenticate

1. POST to `https://platform.qa.com/oauth2/token/` with Basic auth `CLIENT_ID:CLIENT_SECRET`, header `content-type: application/x-www-form-urlencoded`, body `grant_type=client_credentials`.
2. Reuse the returned bearer token for up to 10 hours; send it as `Authorization: Bearer <token>` on every call.

## Steps

1. **List catalog** — call `v2_content_catalog_list` (`GET /v2/content/catalog/`). Page with `page[number]` and `page[size]`. Narrow with `filter[content_type]` (comma-separated: `lp`, `course`, `quiz`, `exam`, `resource`, `lab`, `labchallenge`). Opt into heavier fields with `additional_fields=description,skills`.
2. **Follow pagination** — traverse the `next` link until exhausted.
3. **Fetch one entity** — call `v2_content_get_entity` (`GET /v2/content/entity/`) to retrieve full detail for a specific catalog item.

## Rules

- Rate limit: 100 requests/minute. On HTTP 429, read the `Retry-After` header and back off.
- Errors return a JSON:API `errors[]` envelope: `{ detail, source.pointer, status }` — surface `detail` to the user.
- The v1 catalog operations (`v1_content_catalog_list`) still exist but prefer v2.
