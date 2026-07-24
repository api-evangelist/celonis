---
name: Query a Celonis Knowledge Model
description: Authenticate, discover a Knowledge Model, and read its records, KPIs and query results from the Celonis Process Intelligence API.
api: openapi/celonis-knowledge-model-openapi.yaml
operations: [getKnowledgeModel, getKnowledgeModelDetails, getRecords, getRecordDetails, getKpis, getKnowledgeModelDataResult, getKnowledgeModelQueryResultByUsingQueryInBody]
---

# Query a Celonis Knowledge Model

Read process-intelligence data (records, KPIs, query results) from a Celonis Knowledge Model (KM).

## Auth
- Base URL: `https://{team_domain}.{realm}.celonis.cloud`.
- Send an `Authorization` header. Prefer OAuth 2.0 Bearer JWT (client-credentials); `AppKey <token>` also works but is deprecating. OAuth tokens live ~15 min — refresh before expiry.
- See `authentication/celonis-authentication.yml`.

## Steps
1. **List Knowledge Models** — `getKnowledgeModel` (`GET /intelligence/api/knowledge-models`). Paginate with `page` (from 0) and `pageSize`. Pick your `km_id`.
2. **Inspect the KM** — `getKnowledgeModelDetails` (`GET /intelligence/api/knowledge-models/{km_id}`) to see its records, KPIs and filters.
3. **List records** — `getRecords` (`GET /intelligence/api/knowledge-models/{km_id}/records`); drill into one with `getRecordDetails` (`.../records/{record_id}`).
4. **Read KPIs** — `getKpis` (`GET /intelligence/api/knowledge-models/{km_id}/kpis`).
5. **Run a data query** — `getKnowledgeModelDataResult` (`GET .../data`) for simple reads, or `getKnowledgeModelQueryResultByUsingQueryInBody` (`POST .../data`) for a query in the request body. The `/data` endpoint returns at most 5000 records — page through larger result sets.

## Conventions
- Pagination: `page`/`pageSize` (see `conventions/celonis-conventions.yml`).
- Filtering/sorting uses OData semantics on the KM API.
- Rate limits surface as `x-ratelimit-limit` / `-remaining` / `-reset`; a `429` means wait `x-ratelimit-reset` seconds.
- Errors return `{title, status, detail, errorCode}` (not RFC 9457) — see `errors/celonis-problem-types.yml`.
