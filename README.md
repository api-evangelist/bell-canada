# Bell Canada (bell-canada)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Bell Canada is Canada's largest communications company and the principal operating subsidiary of BCE Inc., providing wireless, wireline, internet, television and enterprise network services across Canada, and operating Bell Media as the country's largest broadcaster. As a facilities-based mobile network operator and broadband carrier it sits at the connectivity layer of the telecom value chain, and its public API posture reflects that.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/bell-canada/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/bell-canada/refs/heads/main/apis.yml)

## Tags

- Telecommunications
- Canada
- Mobile Network Operator
- Broadband
- 5G
- IoT
- TM Forum
- BSS
- OSS
- Network APIs
- CAMARA
- Open Gateway
- Identity Verification
- SIM Swap
- Enterprise

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## The API surface, honestly

Bell runs a real developer portal at [developer.bell.ca](https://developer.bell.ca) (HTTP 200, confirmed 2026-07-25). It is better than a marketing landing page and short of a self-serve developer platform:

- **Documentation is public.** Four API reference pages with operation tables, curl and Python examples, downloadable Swagger 2.0 JSON, and downloadable PDF specifications — all reachable anonymously, no login wall.
- **Access is not.** There is no self-serve signup, no key issuance, no console, no published base URL (every spec declares the host as the literal placeholder `serverRoot`) and no published credential scheme (examples redact it as `SECURITY_CREDENTIALS`). Credentials come from submitting a business registration form that Bell reviews manually.

The entire published surface is BSS/OSS B2B service management aligned to **TM Forum Open APIs**. There is no consumer API, no messaging or voice API, no documented IoT connectivity management API, and no 5G/MEC API despite the portal's 5G and edge computing marketing.

## APIs

### Bell Canada Trouble Ticket API

TM Forum TMF621 v4.1.1 (Bell v2.5). Create, patch, retrieve and list trouble tickets against Bell services from a partner ITSM or fault management system.

- **Human URL:** [https://developer.bell.ca/troubleticket](https://developer.bell.ca/troubleticket)

#### Properties

- [Documentation](https://developer.bell.ca/troubleticket)
- [API Reference](https://developer.bell.ca/troubleticket)
- [OpenAPI](openapi/bell-canada-trouble-ticket-api-openapi.json) — Swagger 2.0
- [Specification (PDF)](https://developer.bell.ca/uploads/BELL_Canada_API_Specification_Trouble_Ticket_v2_5_af537009fa.pdf)

### Bell Canada Service Order API

TM Forum TMF641 v4.6 (Bell v1.4). Place, amend, cancel and track service requests with Bell over a B2B integration.

- **Human URL:** [https://developer.bell.ca/serviceorder](https://developer.bell.ca/serviceorder)

#### Properties

- [Documentation](https://developer.bell.ca/serviceorder)
- [API Reference](https://developer.bell.ca/serviceorder)
- [OpenAPI](openapi/bell-canada-service-order-api-openapi.json) — Swagger 2.0
- [Specification (PDF)](https://developer.bell.ca/uploads/BELL_Canada_API_Specification_Service_Order_v1_4_1_1bdd4073d5.pdf)

### Bell Canada Resource Inventory Management API

TM Forum TMF639 v4.1 (Bell v1.6). Query and maintain an inventory view of the logical and physical Bell resources supporting a partner's services.

- **Human URL:** [https://developer.bell.ca/resourceinventory](https://developer.bell.ca/resourceinventory)

#### Properties

- [Documentation](https://developer.bell.ca/resourceinventory)
- [API Reference](https://developer.bell.ca/resourceinventory)
- [OpenAPI](openapi/bell-canada-resource-inventory-api-openapi.json) — Swagger 2.0
- [Specification (PDF)](https://developer.bell.ca/uploads/BELL_Canada_API_Specification_Resource_Inventory_v1_6_1_855c08df43.pdf)

### Bell Canada Change Management API

TM Forum TMF655 v4.2 (Bell v1.1). Raise, update, retrieve and list change requests against Bell services with guaranteed message delivery.

- **Human URL:** [https://developer.bell.ca/changemanagement](https://developer.bell.ca/changemanagement)

#### Properties

- [Documentation](https://developer.bell.ca/changemanagement)
- [API Reference](https://developer.bell.ca/changemanagement)
- [OpenAPI](openapi/bell-canada-change-management-api-openapi.json) — Swagger 2.0
- [Specification (PDF)](https://developer.bell.ca/uploads/BELL_Canada_API_Specification_Change_Management_v1_1_1_b54db5b465.pdf)

## Events

All four APIs implement the TM Forum hub/listener notification pattern — `POST /hub` registers a callback URL, `DELETE /hub/{id}` removes it, and a `POST /listener/{event}` contract exists per event type (create, attribute value change, state/status change, delete, plus resolved, milestone, jeopardy, approval-required, information-required and failure variants). No AsyncAPI document is published.

## CAMARA and GSMA Open Gateway

Bell publishes **no first-party CAMARA network APIs**. `opengateway.bell.ca`, `developers.opengateway.bell.ca`, `developers.bell.ca` and `docs.bell.ca` do not resolve; `www.bell.ca/opengateway` returns 404.

Bell's network capabilities reach developers through an aggregator instead. **EnStream LP** — the telco identity and fraud-signal joint venture owned by Bell Mobility, Rogers Communications and TELUS Communications — announced a partnership with **Aduna** (the Ericsson-and-carrier joint venture) on **27 February 2025** to bring Canadian **Number Verification** and **SIM Swap** network signals into Aduna's CAMARA-aligned global distribution platform. That is a real, named, dated commercial channel — not a press release with nothing behind it — but the callable surface, the documentation and the developer relationship belong to EnStream and Aduna, not to Bell.

GSMA's Open Gateway supporter pages returned HTTP 403 to anonymous fetches on the review date, so Bell's membership could not be confirmed from a primary source. Bell makes no Open Gateway claim of its own. The 2012 GSMA OneAPI Gateway launched with Bell Mobility, Rogers and TELUS is a different, much older programme and should not be read as Open Gateway participation.

**The sector finding, plainly: Bell owns the network; an aggregator owns the developer.**

## Links

- [Bell Canada](https://www.bell.ca)
- [BCE Inc.](https://www.bce.ca)
- [Bell Developer Portal](https://developer.bell.ca)
- [API Overview](https://developer.bell.ca/overview)
- [Service Management APIs](https://developer.bell.ca/servicemanagement)
- [Register for Enterprise API access](https://developer.bell.ca/register)
- [EnStream](https://enstream.com/)
