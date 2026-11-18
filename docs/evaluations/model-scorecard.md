# Context scorecard — BL-03 overdue service requests

> Score 1 = convention met, 0 = not met. Check with the command, not by eye.

| # | Convention (source) | How to check | Baseline | With context |
|---|---|---|:---:|:---:|
| 1 | Errors via `badRequest` / `notFound` / `unprocessable` ([ADR-0002](../adr/0002-error-envelope.md)) | `grep -nE "res\.end\|statusCode\|error:" api/src/routes/serviceRequests.ts` → no output | | |
| 2 | Static `/overdue` registered **before** `/:id` ([ADR-0003](../adr/0003-layered-architecture.md) rule 3) | `grep -n "router.get('/api/service-requests" api/src/routes/serviceRequests.ts` → `/overdue` line number is lower | | |
| 3 | SLA days 5 / 10 / 14 / 30 from [service-levels.md](../domain/service-levels.md) | `grep -rnE "address-change\|value-transfer\|retirement-quote\|complaint" api/src/domain` → the four values | | |
| 4 | Business rule in `api/src/domain/`, not in the route (ADR-0003 rule 1) | `git status --short api/src/domain` → a new file; the route only imports it | | |
| 5 | Tests use `startTestServer()` + fixed clock (ADR-0003 rule 4) | `grep -c "startTestServer" api/test/<new-file>.test.ts` ≥ 1 and `grep -c "new Date()" …` = 0 | | |
| 6 | `api/openapi.yaml` updated (ADR-0003 rule 2) | `grep -n "overdue" api/openapi.yaml` → a path entry | | |
| 7 | Erasable TS only: `.ts` imports, `import type`, no `enum` ([ADR-0001](../adr/0001-zero-dependency-runtime.md)) | `grep -rnwE "enum\|namespace" api/src api/test` → no output | | |
| 8 | `npm test` green | `npm test` → `fail 0` | | |
| | **Total** | | **_ / 8** | **_ / 8** |

## Observations

| Run | Time to "done" | What happened (1–2 lines) |
|---|---|---|
| Baseline | | |
| With context | | |