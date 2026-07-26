---
name: Raise a change request against a Bell service
description: Submit a change request with a planned window, risk and impact, carry approvals through notifications, and link it to the trouble ticket that caused it.
api: openapi/bell-canada-change-management-api-openapi.json
standard: TM Forum TMF655 v4.2 (Bell v1.1)
operations:
  - createChangeRequest
  - retrieveChangeRequest
  - listChangeRequest
  - patchChangeRequest
  - registerListener
  - unregisterListener
generated: '2026-07-25'
method: generated
source: openapi/bell-canada-change-management-api-openapi.json + https://developer.bell.ca/changemanagement
---

# Raise a change request against a Bell service

## Before you start

- Partner-gated: register at <https://developer.bell.ca/register>, endpoint and credentials by email. `host` is `serverRoot` in the published Swagger.
- Credential header published as the placeholder `SECURITY_CREDENTIALS`; `x-external-system` (8+ unique chars) in sandbox.
- No idempotency key — set your own correlation and never blind-retry `createChangeRequest`.

## Steps

1. **Raise the change — `createChangeRequest`** (`POST /changeRequest`).
   The TMF655 `ChangeRequest` is the richest entity Bell publishes. Populate:
   - **What**: `description`, `requestType`, `channel`, `changeRequestCharacteristic[]`
   - **When**: `requestDate`, `scheduledDate`, `plannedStartTime`, `plannedEndTime`
   - **Risk posture**: `priority`, `impact`, `risk`, `riskValue`, `riskMitigationPlan`
   - **Blast radius**: `impactEntity[]` and `targetEntity[]` — the services and entities the change touches
   - **Commercial / contractual**: `budget` (a `Money`), `sla[]` (`SLARef`), `specification` (`EntitySpecificationRef`), `location`
   - **Provenance**: `troubleTicket[]` (`TroubleTicketRef`) to link the change back to the fault that triggered it, and `problemTicket[]` (`ServiceProblemRef`)
   Expect `201` or `202`.
2. **Watch for the approval gate — `registerListener`** (`POST /hub`) with `{id, callback, query}`. Bell POSTs six events:
   - `changeRequestCreateEvent`
   - `changeRequestStatusChangeEvent`
   - `changeRequestAttributeValueChangeEvent`
   - **`changeRequestApprovalRequiredEvent`** — the one that matters. A change sits until it is approved; wire this event to whoever can approve, not to a log file.
   - `changeRequestFailureEvent`
   - `changeRequestDeleteEvent`
3. **Read — `retrieveChangeRequest`** (`GET /changeRequest/{id}`) or **`listChangeRequest`** (`GET /changeRequest`) with `offset` / `limit` / `sort` / `fields` / `expand` / `depth`, paging on `X-Result-Count` and `X-Total-Count`.
4. **Progress it — `patchChangeRequest`** (`PATCH /changeRequest/{id}`): add `workLog[]` entries, move `status` (a `ChangeRequestStatusType`), record `actualStartTime` / `actualEndTime`, attach the `resolution`. Send only changed fields.
5. **Stop listening — `unregisterListener`** (`DELETE /hub/{id}`).

## Notes

- Bell's page claims *"guaranteed message delivery"* for create and patch. That is a Bell-side delivery assurance, **not** a client-side retry-safety contract — there is still no idempotency key.
- `problemTicket` references TMF656 Service Problem, an API Bell does not publish, so that link can only carry an identifier from another system.

## Errors

TM Forum `Error` envelope (`code`, `reason`, optional `message` / `status` / `referenceError`) on 400 / 401 / 403 / 404 / 405 / 409 / 422 / 500. See `errors/bell-canada-problem-types.yml`.
