---
name: Query and maintain the Bell resource inventory
description: Read logical, physical and generic resource records backing your Bell services, and keep an external CMDB in step with them.
api: openapi/bell-canada-resource-inventory-api-openapi.json
standard: TM Forum TMF639 v4.1 (Bell v1.6)
operations:
  - listResource
  - retrieveResource
  - listLogicalResource
  - retrieveLogicalResource
  - listPhysicalResource
  - retrievePhysicalResource
  - patchResource
  - executePatchResource
  - registerListener
  - unregisterListener
generated: '2026-07-25'
method: generated
source: openapi/bell-canada-resource-inventory-api-openapi.json + https://developer.bell.ca/resourceinventory
---

# Query and maintain the Bell resource inventory

## Before you start

- Partner-gated. Endpoint and credentials arrive by email after registration approval at <https://developer.bell.ca/register>; `host` in the Swagger document is `serverRoot`.
- Bell's reference page documents only `GET /Resource` and `GET /Resource/{id}`. The Swagger document specifies far more — the logical and physical collections, POST, PUT, PATCH, DELETE and the JSON Patch route. **Treat anything beyond the two documented reads as specified-but-unconfirmed and check with your Bell representative before relying on it.**
- No idempotency key on the write operations.

## The three collections

| Collection | Entity | What it holds |
|---|---|---|
| `/resource` | `Resource` | The generic TMF639 resource — `name`, `description`, `category`, `resourceVersion`, `startOperatingDate`, `endOperatingDate`, four state fields (`resourceStatus`, `administrativeState`, `operationalState`, `usageState`), `resourceCharacteristic[]`, `resourceSpecification`, `place`. |
| `/logicalResource` | `LogicalResource` | `Resource` plus `value` — identifiers, addresses, circuits. |
| `/physicalResource` | `PhysicalResource` | `Resource` plus `serialNumber`, `versionNumber`, `manufactureDate`, `powerState` — equipment. |

## Steps

1. **Search — `listResource`** (`GET /resource`), **`listLogicalResource`** (`GET /logicalResource`) or **`listPhysicalResource`** (`GET /physicalResource`).
   Page with `offset` and `limit`; read `X-Result-Count` and `X-Total-Count` from the response headers. Trim the payload with `fields`, and pull nested objects inline with `expand` plus `depth` rather than issuing N follow-up reads.
2. **Read one — `retrieveResource` / `retrieveLogicalResource` / `retrievePhysicalResource`** (`GET /{collection}/{id}`).
3. **Sync your CMDB by subscribing — `registerListener`** (`POST /hub`) with `{id, callback, query}`. Bell POSTs eight events: `resourceCreateEvent`, `resourceAttributeValueChangeEvent`, `resourceStateChangeEvent`, `resourceDeleteEvent` and the four `physicalResource*` equivalents. Use `query` to filter. This is the right way to keep an inventory in step — a nightly full list is not.
4. **Update — `patchResource`** (`PATCH /resource/{id}`), `patchLogicalResource`, `patchPhysicalResource`. Send only the fields that change.
5. **Apply a JSON Patch document — `executePatchResource`** (`PATCH /logicalResource/executeJSONPatch`). This is the one RFC 6902 JSON Patch route in Bell's whole published surface, and it exists only on the logical-resource collection.
6. **Stop listening — `unregisterListener`** (`DELETE /hub/{id}`).

## Errors

TM Forum `Error` envelope on 400 / 401 / 403 / 404 / 405 / 409 / 422 / 500 — see `errors/bell-canada-problem-types.yml`. There is no 429 and no published rate limit, so throttle yourself: batch through `limit` rather than firing parallel reads.
