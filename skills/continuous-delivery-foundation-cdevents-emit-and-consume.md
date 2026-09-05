---
name: Emit and consume CDEvents
description: >-
  Build a valid CDEvent against the published JSON Schemas, wrap it in the normative CloudEvents
  1.0 binding, and deliver it to a receiver that accepts one — Spinnaker's CDEvents webhook.
api: json-schema/cdevents/
generated: '2026-09-05'
method: generated
source: >-
  json-schema/cdevents/ (49 harvested schemas),
  https://github.com/cdevents/spec/blob/main/cloudevents-binding.md,
  openapi/continuous-delivery-foundation-spinnaker-openapi.json
operations:
  - webhooks_1
  - preconfiguredWebhooks
---

# Emit and consume CDEvents

CDEvents is the Continuous Delivery Foundation's vocabulary for CI/CD events. It is not an API —
it is 49 JSON Schema 2020-12 documents plus a transport binding. Both are harvested into this
repository.

## Pick the event type

Every event type is `dev.cdevents.<subject>.<predicate>.<major.minor.patch>`, and every schema
pins its own type as a single-value enum. The subjects are:

`pipelineRun`, `taskRun`, `build`, `artifact`, `repository`, `branch`, `change`, `environment`,
`service`, `testCaseRun`, `testSuiteRun`, `testOutput`, `incident`, `ticket`, `approval`.

The full list with each schema's `$id` and exact type string is in
`asyncapi/continuous-delivery-foundation-cdevents-events.yml`. **Read the type off the schema.**
Do not compose one from the naming pattern — event versions differ per subject (artifact events
are at `0.2.0` while approval events are at `0.1.0`).

## Build the event

Validate against the harvested schema for your type, e.g.
`json-schema/cdevents/continuous-delivery-foundation-cdevents-pipelinerunstarted.json`. Every
event has the same four top-level members: `context`, `subject`, `customData`,
`customDataContentType`.

**Version your pin deliberately.** The harvested schemas carry `$id` values under
`https://cdevents.dev/0.6.0-draft/schema/...` — they are ahead of the last tagged release,
`v0.5.1` (2026-04-15). If you need released stability, read the schemas from the `v0.5.1` tag of
`github.com/cdevents/spec` instead of from `main`.

## Wrap it for the wire

`cloudevents-binding.md` is normative: the CloudEvents `specversion` **MUST** be `1.0`, and the
CDEvents context maps onto the CloudEvents `id`, `source`, `type`, `subject`, `time`,
`datacontenttype` and `dataschema` attributes. A consumer that already speaks CloudEvents needs no
bespoke connector, which is the whole point of the binding.

## Deliver it

Spinnaker is the one CDF project whose published contract accepts CDEvents:

- `webhooks_1` — `POST /webhooks/cdevents/{source}`, request body typed as `CloudEvent`. Its own
  summary: *"Endpoint for posting webhooks to Spinnaker's CDEvents webhook service."*
- `preconfiguredWebhooks` — `GET /webhooks/preconfigured`, to see what the operator has wired up.

Neither Jenkins nor Screwdriver declares a CDEvents endpoint in any published contract.

## SDKs, and their state

`packages/continuous-delivery-foundation-packages.yml` has the detail. The short version: the Go
SDK (`v0.5.1`, 2026-06-25) and the Rust crate (`0.4.1`, 2026-06-25) are current. The Java SDK last
shipped `0.3.1` in **March 2024**. The Python SDK has never been published to PyPI and the .NET
repository is empty. In Python or .NET, validate against the harvested schemas directly rather
than reaching for a package that is not there.
