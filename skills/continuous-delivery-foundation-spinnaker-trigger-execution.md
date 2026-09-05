---
name: Trigger and track a Spinnaker pipeline execution
description: >-
  Invoke a Spinnaker pipeline through the Gate API, follow the execution, pause, resume, restart a
  failed stage, or cancel it — using the operations that are NOT deprecated.
api: openapi/continuous-delivery-foundation-spinnaker-openapi.json
generated: '2026-09-05'
method: generated
source: openapi/continuous-delivery-foundation-spinnaker-openapi.json
operations:
  - getAllApplications
  - getApplication
  - getPipelineConfigsForApplication
  - invokePipelineConfig
  - getPipelines
  - getPipeline
  - getFailedStagesForPipelineExecution
  - pausePipeline
  - resumePipeline
  - restartStage
  - cancelPipeline
  - searchForPipelineExecutionsByTrigger
---

# Trigger and track a Spinnaker execution

Gate is Spinnaker's API gateway. The contract published at
`https://spinnaker.io/docs/reference/api/swagger.json` covers 288 operations.

## Before you start

- **There is no hosted Spinnaker.** `servers[]` in the published contract is the generated
  placeholder `http://localhost` because Gate runs in the operator's own cluster. Point at your
  own Gate host.
- **The contract declares no securitySchemes.** Authentication is configured per deployment —
  OAuth 2.0, SAML, LDAP or x509 client certificates are all documented deployment options. Ask the
  operator which one is live; do not assume a bearer token.
- **Twelve operations are marked `deprecated: true`.** Four of them are on this exact flow. Use
  the replacements below, not the `/applications/{application}/...` variants.

| Deprecated | Use instead |
|---|---|
| `invokePipelineConfig_1` | `invokePipelineConfig` |
| `cancelPipeline_1` | `cancelPipeline` |
| `task_1`, `getTask_1`, `cancelTask_1` | `task`, `getTask`, `cancelTask` |

## Steps

1. **Find the application.** `getAllApplications`, then `getApplication` for details.
2. **List its pipeline configs.** `getPipelineConfigsForApplication`.
3. **Trigger.** `invokePipelineConfig` (`POST /pipelines/{application}/{pipelineNameOrId}`). There
   is **no idempotency key** — a retried trigger starts a second execution. Before retrying a call
   that timed out, look for the execution with `searchForPipelineExecutionsByTrigger`, which sorts
   newest-first by trigger time.
4. **Follow it.** `getPipelines` lists an application's executions; `getPipeline` reads one by id.
   Poll — no streaming surface is published.
5. **Diagnose a failure.** `getFailedStagesForPipelineExecution` returns only the failed stages and
   traverses nested pipeline executions for you. Reach for this before pulling the whole execution.
6. **Intervene.** `pausePipeline` / `resumePipeline` hold and release an execution;
   `restartStage` re-runs one stage; `cancelPipeline` stops the execution. None of these states a
   time window — they are valid while the execution is live.

## Optional: label your traffic

47 operations accept an optional `X-RateLimit-App` **request** header. It is how a caller
identifies itself so the operator's rate limiting can attribute the call. Send a stable value for
your agent. Nothing is returned in reply — Spinnaker publishes no rate-limit response headers and
no documented limit.

## Errors

**The contract declares no 4xx or 5xx response on any operation.** Nothing about Spinnaker's
failure modes can be read from the spec, and there is no public instance to probe. Treat any
non-2xx as opaque, log the body verbatim, and do not build error handling on an assumed shape.
