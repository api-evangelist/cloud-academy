---
name: Manage organization teams and members
description: Create QA (Cloud Academy) organization teams, read them, and add or remove members within a team.
api: openapi/cloud-academy-openapi-original.json
operations: [v1_organizations_accounts_teams_create, v1_organizations_accounts_teams_read, v1_organizations_accounts_teams_list, v1_organizations_accounts_teams_members_create, v1_organizations_accounts_teams_members_delete, v1_organizations_accounts_teams_list_subteams]
---

# Manage organization teams and members

Manage the org structure of a QA (Cloud Academy) enterprise account. Requires an Enterprise admin API key pair.

## Authenticate

POST `https://platform.qa.com/oauth2/token/` with Basic auth `CLIENT_ID:CLIENT_SECRET` and body `grant_type=client_credentials`; use the bearer token (10-hour lifetime) on every call.

## Steps

1. **List existing teams** — `v1_organizations_accounts_teams_list` (`GET /v1/organizations/accounts/teams/`).
2. **Create a team** — `v1_organizations_accounts_teams_create` (`POST /v1/organizations/accounts/teams/`).
3. **Read a team** — `v1_organizations_accounts_teams_read` (`GET /v1/organizations/accounts/teams/{id}/`).
4. **List sub-teams** — `v1_organizations_accounts_teams_list_subteams` (`GET /v1/organizations/accounts/teams/{id}/subteams/`).
5. **Add members** — `v1_organizations_accounts_teams_members_create` (`POST /v1/organizations/accounts/teams/{id}/members/`).
6. **Remove members** — `v1_organizations_accounts_teams_members_delete` (`DELETE /v1/organizations/accounts/teams/{id}/members/`).

## Rules

- Writes (POST/DELETE) are consequential — confirm intent before mutating team membership; there is no idempotency-key, so avoid blind retries on ambiguous responses.
- Rate limit: 100 requests/minute; honor `Retry-After` on HTTP 429.
- Errors are JSON:API `errors[]` — a 404 on `{id}` means the team does not exist or is not in your account.
