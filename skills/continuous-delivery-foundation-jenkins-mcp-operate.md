---
name: Operate Jenkins through its MCP server
description: >-
  Connect an agent to a Jenkins controller running the official MCP Server plugin, then find jobs,
  trigger builds, read logs with cursor pagination, and diagnose failures.
api: mcp/continuous-delivery-foundation-mcp.yml
generated: '2026-09-05'
method: generated
source: >-
  https://github.com/jenkinsci/mcp-server-plugin (README, Available Tools),
  https://plugins.jenkins.io/mcp-server/
operations:
  - getJobs
  - getJob
  - triggerBuild
  - getQueueItem
  - getBuild
  - getBuildLog
  - searchBuildLog
  - getTestResults
  - getBuildChangeSets
  - rebuildBuild
  - getStatus
  - whoAmI
---

# Operate Jenkins through MCP

This is the only MCP server in the CDF estate and it is **self-hosted** — it runs inside the
operator's own Jenkins controller. There is no vendor endpoint. The tool names below are
transcribed from the plugin's own documentation; call `tools/list` against your controller to get
the input schemas, which are not published anywhere static.

## Connect

- Install the **MCP Server** plugin (`mcp-server`) from the Jenkins Plugin Manager. No further
  configuration is required.
- Endpoint: `https://<your-jenkins-host>/mcp-server/mcp` (Streamable HTTP — the transport the
  plugin recommends). SSE lives at `/mcp-server/sse` and a session-less variant at
  `/mcp-server/stateless`.
- Auth: HTTP **Basic** with a Jenkins username and a Jenkins **API token** (User → Security → Add
  new token). The server acts as that user and inherits that user's Jenkins permissions — the tool
  surface an agent sees is the permission set you give the token, so scope it deliberately.
- `/mcp-health` needs no auth and returns 200 healthy, 503 while shutting down, with `Retry-After`.
  Poll it to decide when to reconnect.

## Orient before acting

1. `getStatus` — health and readiness of the controller. The plugin's docs say to use this rather
   than a simple up/down check.
2. `whoAmI` — confirm which user the token maps to, and therefore what you are allowed to do.
3. `getJobs` — paginated, sorted by name. `getJob` reads one by full path.

## Trigger and follow a build

4. `triggerBuild` — takes `jobFullName` and a `parameters` object. It returns a **queue item**,
   not a build. String, boolean, choice, text, password and run parameters are supported; **file
   parameters are not supported over MCP**.
5. `getQueueItem` — pass the returned id to find out whether the queue item became a build.
6. `getBuild` — the build itself, or the last build of a job.

`triggerBuild` has **no idempotency key**. Calling it twice queues two builds. If the call fails
without a queue item, check `getJob` for a new build before retrying.

## Read logs without drowning

`getBuildLog` is the best-designed thing on this surface. It supports forward reads,
**end-relative** reads (negative `skip`/`limit` — use this to get the tail of a failure), and
cursor pagination: every response carries `nextCursor`, which you pass back as `cursor`.

- The cursor is bound to the `(job, buildNumber)` it was issued for and is rejected against
  another build.
- `totalLines` is exact only for end-relative reads; it is `-1` for forward and cursor reads.
- If `nextCursor` is set but `hasMoreContent` is false, the build is still running and you have
  read everything written so far — keep the cursor and call again later.

For a known failure signature, `searchBuildLog` matches a string or regex and stops early once
`maxMatches` is reached. Prefer it over paging the whole log.

## Diagnose

`getTestResults` for the test report, `getBuildChangeSets` for what changed in this build,
`getBuildScm` / `getJobScm` for where the code came from, `findJobsWithScmUrl` to find every job
built from one repository.

## Re-run, carefully

`rebuildBuild` re-runs with the same parameters and returns a new queue item. For Pipeline jobs,
`getReplayScripts` then `replayBuild` lets you re-run with a **modified** script — that is an
arbitrary-code path into the controller, gated by Jenkins permissions and the script sandbox.
Treat it as the highest-consequence tool on this server.
