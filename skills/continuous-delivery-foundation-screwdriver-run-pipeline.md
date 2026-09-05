---
name: Run a Screwdriver pipeline and follow it to completion
description: >-
  Authenticate to the Screwdriver v4 API, find a pipeline, start an event, follow the builds it
  produces, read a step's logs, and stop the event if it must be aborted.
api: openapi/continuous-delivery-foundation-screwdriver-openapi.json
base_url: https://api.screwdriver.cd/
generated: '2026-09-05'
method: generated
source: openapi/continuous-delivery-foundation-screwdriver-openapi.json
operations:
  - getV4AuthContexts
  - getV4Pipelines
  - getV4PipelinesId
  - getV4PipelinesIdJobs
  - postV4Events
  - getV4EventsId
  - getV4EventsIdBuilds
  - getV4BuildsId
  - getV4BuildsIdSteps
  - getV4BuildsIdStepsNameLogs
  - putV4EventsIdStop
---

# Run a Screwdriver pipeline

Screwdriver models a run as an **event** that fans out into **builds**, one per job. You do not
start a build directly for a normal run — you create an event on a pipeline.

## Before you start

- Base URL is `https://api.screwdriver.cd/` for the project-operated cluster, or your own
  Screwdriver host. Every path is prefixed `/v4/`.
- Auth is a **JWT in the `Authorization` header** — the contract declares one security scheme,
  `jwt` (`apiKey`, `in: header`, name `Authorization`), applied globally. Call
  `getV4AuthContexts` to see which login contexts the instance offers.
- There is **no idempotency key**. `postV4Events` is not safe to retry blindly — a repeated call
  starts a second run. If a call times out, look for the event with `getV4PipelinesIdEvents`
  before retrying.

## Steps

1. **Find the pipeline.** `getV4Pipelines` — paginated with `page`, `count`, `sort`, `sortBy` and
   `search`. Ask for the smallest page you need; there is no cursor.
2. **Read it.** `getV4PipelinesId` for the pipeline record, `getV4PipelinesIdJobs` for the jobs
   the event will fan out into.
3. **Start the run.** `postV4Events` with the pipeline id and the job to start. This is the write.
   Record the returned event id before doing anything else.
4. **Watch the event.** `getV4EventsId` for status, `getV4EventsIdBuilds` for the builds it
   created. Poll on a backoff — no webhook or streaming surface is published for this.
5. **Read a build.** `getV4BuildsId`, then `getV4BuildsIdSteps` for the step list and
   `getV4BuildsIdStepsNameLogs` for one step's log output.
6. **Abort if you must.** `putV4EventsIdStop` stops all builds in the event. This is the only
   reversal operation on this flow and the docs state **no window** — it works while the event is
   running and not after.

## Errors

Screwdriver returns the hapi/Boom envelope, not RFC 9457:

```json
{"statusCode":401,"error":"Unauthorized","message":"Missing authentication"}
```

The contract declares **no 4xx or 5xx responses on any of its 152 operations**, so treat the
envelope above as the contract — it was observed live, not read from the spec. A `404` on a route
you believe exists is far more likely a missing `/v4/` prefix than a missing resource.

## What you cannot do

`deleteV4PipelinesId` and `deleteV4SecretsId` have **no restore operation**. There is no undelete
anywhere in this API. Treat every DELETE as terminal.
