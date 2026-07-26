---
name: Place, amend and cancel a Bell service order
description: Submit a provisioning request to Bell, track milestones and jeopardy alerts through notifications, and cancel an in-flight order.
api: openapi/bell-canada-service-order-api-openapi.json
standard: TM Forum TMF641 v4.6 (Bell v1.4)
operations:
  - createServiceOrder
  - patchServiceOrder
  - retrieveServiceOrder
  - listServiceOrder
  - createCancelServiceOrder
  - retrieveCancelServiceOrder
  - listCancelServiceOrder
  - registerListener
  - unregisterListener
generated: '2026-07-25'
method: generated
source: openapi/bell-canada-service-order-api-openapi.json + https://developer.bell.ca/serviceorder
---

# Place, amend and cancel a Bell service order

## Before you start

- Partner-gated, same as every Bell API: register at <https://developer.bell.ca/register>, and Bell emails the endpoint and credentials after manual approval. `host` in the Swagger document is the placeholder `serverRoot`.
- Credential header is published only as the placeholder `SECURITY_CREDENTIALS`; add `x-external-system` (8+ unique chars) in sandbox.
- **Read this first:** Bell's own reference page marks `GetServiceOrder` (`retrieveServiceOrder`) and `GetServiceOrderList` (`listServiceOrder`) as *"Currently not available; will be implemented in the near future"* — even though both are fully specified in the Swagger document. **Do not design a read-back polling loop on this API.** Use notifications (step 3) as the primary status channel.
- No idempotency key. A retried `createServiceOrder` can duplicate a provisioning request — an expensive mistake here. Always set your own `externalId` and reconcile before retrying.

## Steps

1. **Place the order — `createServiceOrder`** (`POST /serviceOrder`).
   Set `category`, `description`, `priority`, `externalId`, `requestedStartDate` / `requestedCompletionDate`, `notificationContact`, the `relatedParty[]` and one or more `serviceOrderItem[]` describing what is being provisioned. Expect `201` or `202`.
2. **If 202, poll the monitor.** `GET /monitor/{monitorId}` is documented on the reference page for long-running transactions. It is docs-only — absent from the Swagger document.
3. **Subscribe to the order lifecycle — `registerListener`** (`POST /hub`) with an `EventSubscription` `{id, callback, query}`. Bell will POST:
   - `serviceOrderCreateEvent`, `serviceOrderStateChangeEvent`, `serviceOrderAttributeValueChangeEvent`, `serviceOrderDeleteEvent`
   - `serviceOrderMilestoneEvent` — provisioning milestones
   - `serviceOrderJeopardyEvent` — Bell signalling the order is at risk of missing its date
   - `serviceOrderInformationRequiredEvent` — Bell needs something from you; **act on this or the order stalls**
   - `serviceOrderFailureEvent`
   Given that read-back is not available, these events are the order status.
4. **Amend — `patchServiceOrder`** (`PATCH /serviceOrder/{id}`). Bell: *"Partner can send an Update request for an existing Service Request into Bell (by Bell SR ID) ... used to update allowable SR details such as worklog."* Send only changed fields.
5. **Cancel — `createCancelServiceOrder`** (`POST /cancelServiceOrder`).
   Cancellation is its own resource, not a state change on the order. Body carries `serviceOrder` (a `ServiceOrderRef`), `cancellationReason` and `requestedCancellationDate`. Track it with `retrieveCancelServiceOrder` (`GET /cancelServiceOrder/{id}`) or `listCancelServiceOrder`, and via `cancelServiceOrderCreateEvent`, `cancelServiceOrderStateChangeEvent` and `cancelServiceOrderInformationRequiredEvent`. Its `state` is a `TaskStateType`, and `errorMessage` explains a rejection.
6. **Stop listening — `unregisterListener`** (`DELETE /hub/{id}`).

## Errors

TM Forum `Error` envelope (`code`, `reason`, optional `message` / `status` / `referenceError`) on 400, 401, 403, 404, 405, 409, 422 and 500. Full table in `errors/bell-canada-problem-types.yml`. Never blind-retry a create or a cancel.
