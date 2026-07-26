---
name: Raise and track a Bell trouble ticket
description: Open a fault against a Bell service from an ITSM system, poll or subscribe for status, and close the loop when Bell resolves it.
api: openapi/bell-canada-trouble-ticket-api-openapi.json
standard: TM Forum TMF621 v4.1.1 (Bell v2.5)
operations:
  - createTroubleTicket
  - retrieveTroubleTicket
  - listTroubleTicket
  - patchTroubleTicket
  - registerListener
  - unregisterListener
generated: '2026-07-25'
method: generated
source: openapi/bell-canada-trouble-ticket-api-openapi.json + https://developer.bell.ca/troubleticket
---

# Raise and track a Bell trouble ticket

## Before you start

- **You cannot call this API without a partner relationship.** Bell publishes no base URL — the Swagger document declares `host: serverRoot`. Register at <https://developer.bell.ca/register>; a Bell administrator reviews the request and emails you a user ID, the API endpoint and credentials. Do not guess a host.
- Send the credential in the header Bell issues you. Every published example redacts it as the literal placeholder `SECURITY_CREDENTIALS`.
- In sandbox, also send `x-external-system` with any unique value of 8 or more characters.
- `Content-Type` and `Accept` are `application/json;charset=utf-8`.
- **There is no idempotency key.** A retried `createTroubleTicket` can create a second ticket. Correlate on your own `externalId` before retrying, and treat every write as at-most-once.

## Steps

1. **Open the ticket — `createTroubleTicket`** (`POST /troubleTicket`).
   Populate `description`, `ticketType`, `severity`, `priority`, your own `externalId` for correlation, and the `relatedEntity[]` / `relatedParty[]` arrays identifying the affected Bell service and the contacts. Every nested object takes the TM Forum `@type` / `@baseType` triple.
   Expect `201 Created`, or `202 Accepted` when Bell runs the create as a long-running transaction.
2. **If you get 202, poll the monitor.** Bell documents `GET /monitor/{monitorid}` (the TMF TransactionMonitor pattern) on the reference page for exactly this case. Note that this route is **not in the Swagger document** — it is docs-only, so treat its shape as unversioned.
3. **Subscribe instead of polling — `registerListener`** (`POST /hub`).
   Body is an `EventSubscription`: `{ "id": "...", "callback": "https://your-host/bell/events", "query": "..." }`. Use `query` for TM Forum event filtering. Bell then POSTs to your callback for `troubleTicketCreateEvent`, `troubleTicketAttributeValueChangeEvent`, `troubleTicketStatusChangeEvent`, `troubleTicketResolvedEvent`, `troubleTicketInformationRequiredEvent` and `troubleTicketDeleteEvent`.
   **Bell publishes no callback signature, shared secret or replay window** — authenticate the callback on your own side (mTLS, allow-list, or a secret in the callback path).
4. **Read one ticket — `retrieveTroubleTicket`** (`GET /troubleTicket/{id}`) using the Bell ticket id, or **search — `listTroubleTicket`** (`GET /troubleTicket`) with `offset`, `limit`, `sort`, `fields`, `expand` and `depth`. Read `X-Result-Count` and `X-Total-Count` from the response headers to page.
5. **Update — `patchTroubleTicket`** (`PATCH /troubleTicket/{id}`). Bell is explicit: *"the customer should only be sending fields that need to be changed/updated and not the entire payload."* Never PATCH a whole ticket back.
6. **When you are done listening — `unregisterListener`** (`DELETE /hub/{id}`).

## Errors

Every operation returns the TM Forum `Error` envelope on `application/json` — **not** RFC 9457 problem+json. Required members are `code` and `reason`; `message`, `status` and `referenceError` are optional.

| Status | Meaning | What to do |
|---|---|---|
| 400 | Bad Request | Fix the payload; the TMF envelope's `reason` is safe to show a user. |
| 401 | Unauthorized | Credential header missing or wrong. Credentials are issued by email, not self-serve. |
| 403 | Forbidden | Your partner account is not entitled to this resource. |
| 404 | Not Found | Wrong Bell ticket id. |
| 409 | Conflict | Concurrent update; re-read then re-patch. |
| 422 | Unprocessable Entity | Payload parsed but violates TMF621 constraints. |
| 500 | Internal Server Error | Bell side. There is **no** published retry or rate-limit policy — back off conservatively and never blind-retry a create. |

## What this API does not do

No OAuth, no scopes, no rate-limit headers, no status page, no SLA, no deprecation policy. See `conventions/bell-canada-conventions.yml` and `lifecycle/bell-canada-lifecycle.yml`.
