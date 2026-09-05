---
name: Hold a Spinnaker Managed Delivery rollout with pins and vetoes
description: >-
  Stop a bad artifact version from promoting through Spinnaker Managed Delivery — pause an
  application, pin an environment to a known-good version, veto a bad one, and reverse each of
  those actions.
api: openapi/continuous-delivery-foundation-spinnaker-openapi.json
generated: '2026-09-05'
method: generated
source: openapi/continuous-delivery-foundation-spinnaker-openapi.json
operations:
  - getApplicationDetails
  - getConfigBy
  - getConstraintState
  - updateConstraintStatus
  - pauseApplication
  - resumeApplication
  - createPin
  - deletePin
  - veto
  - markBad
  - markGood
  - deleteVeto
---

# Hold a Managed Delivery rollout

Managed Delivery (Keel) is the declarative half of Spinnaker: you describe the desired state in a
delivery config and it promotes artifact versions through environments. This skill is the set of
brakes — and, unusually for this estate, **every brake here has a documented release**.

## Read the state first

1. `getApplicationDetails` — managed detail for the application.
2. `getConfigBy` — the delivery config, which lists the environments.
3. `getConstraintState` — up to `{limit}` current constraint states for one environment. This is
   what tells you whether a promotion is blocked and why.

## The three brakes, and how to let each one off

| Brake | Apply | Release |
|---|---|---|
| Pause the whole application | `pauseApplication` | `resumeApplication` |
| Pin an environment to a version | `createPin` | `deletePin` |
| Veto a specific artifact version | `veto` or `markBad` | `deleteVeto` or `markGood` |

Notes that matter:

- `deletePin` unpins **everything** in the environment unless you pass the `reference` parameter —
  the operation description says so explicitly. Always pass `reference` when you pinned one
  artifact.
- `markGood` is the same reversal as `deleteVeto`, reached from the UI's vocabulary. Either works.
- **No window is stated for any of these.** They are valid while the state exists. Do not assume a
  pin or veto expires on its own.

## Manual constraint approval

`updateConstraintStatus` sets the status of an environment constraint — this is the operation
behind a manual-judgment approval gate. It is a WRITE with real consequences: approving a
constraint releases a promotion into that environment. There is no idempotency key and no dry run.
Read `getConstraintState` immediately before and after.

## Deleting is not reversible

`deleteManifest` and `deleteManifestByApp` remove a delivery config. There is no restore operation
in the contract. Everything above is a hold; these two are not.
