# Nursa

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

Nursa is a healthcare staffing marketplace, founded in Salt Lake City in 2019, that connects credentialed per diem (PRN) clinicians — registered nurses, licensed practical nurses, certified nursing assistants, certified medication aides and allied professionals — directly with hospitals, skilled nursing facilities, long-term and post-acute care providers, removing the traditional staffing agency from the middle. Facilities post open shifts and set their own rates, clinicians browse and request shifts, and both sides settle through Nursa's shift-report flow. Nursa publishes a Public API V2 (REST, JWT bearer, with an OpenID Connect / OAuth 2.0 authorization server) so workforce-management and scheduling platforms can post shifts, review requests, schedule clinicians, read clinician credentials and subscribe to webhooks — the integration surface behind its Covr, StaffLion, Dropstat, Maple and UKG partnerships. The company is accredited by The Joint Commission and reports more than 4,500 facilities and 500,000 clinicians nationwide.

**Areas:** Company · Healthcare · Health · Staffing · Nursing · Marketplace · Workforce Management · Scheduling · Human Resources · Per Diem · Shifts · Webhook

## Nursa on APIs.io

Everything profiled for Nursa is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/nursa/](https://apis.io/providers/nursa/)** — or go straight to one of the APIs or properties below.

### APIs (14)

- [Nursa Authorization Server](https://apis.io/apis/nursa/auth/) — Nursa's OpenID Connect / OAuth 2.0 authorization server. Issues the JWT access tokens the Nursa Public API requires, supports the authorization code (with…
- [Nursa Clinicians API](https://apis.io/apis/nursa/nursa-clinicians-api/) — The Clinicians API from Nursa — 4 operation(s) for clinicians.
- [Nursa Downloads API](https://apis.io/apis/nursa/nursa-downloads-api/) — The Downloads API from Nursa — 1 operation(s) for downloads.
- [Nursa Facilities API](https://apis.io/apis/nursa/nursa-facilities-api/) — The Facilities API from Nursa — 8 operation(s) for facilities.
- [Nursa Facilities webhooks API](https://apis.io/apis/nursa/nursa-facilities-webhooks-api/) — The Facilities webhooks API from Nursa — 2 operation(s) for facilities webhooks.
- [Nursa Licenses API](https://apis.io/apis/nursa/nursa-licenses-api/) — The Licenses API from Nursa — 1 operation(s) for licenses.
- [Nursa Marketplace API](https://apis.io/apis/nursa/nursa-marketplace-api/) — The Marketplace API from Nursa — 6 operation(s) for marketplace.
- [Nursa Scheduled shifts API](https://apis.io/apis/nursa/nursa-scheduled-shifts-api/) — The Scheduled shifts API from Nursa — 2 operation(s) for scheduled shifts.
- [Nursa Shift reports API](https://apis.io/apis/nursa/nursa-shift-reports-api/) — The Shift reports API from Nursa — 4 operation(s) for shift reports.
- [Nursa Shift requests API](https://apis.io/apis/nursa/nursa-shift-requests-api/) — The Shift requests API from Nursa — 2 operation(s) for shift requests.
- [Nursa Shifts API](https://apis.io/apis/nursa/nursa-shifts-api/) — The Shifts API from Nursa — 1 operation(s) for shifts.
- [Nursa Support API](https://apis.io/apis/nursa/nursa-support-api/) — The Support API from Nursa — 1 operation(s) for support.
- [Nursa User webhooks API](https://apis.io/apis/nursa/nursa-user-webhooks-api/) — The User webhooks API from Nursa — 2 operation(s) for user webhooks.
- [Nursa Webhook logs API](https://apis.io/apis/nursa/nursa-webhook-logs-api/) — The Webhook logs API from Nursa — 1 operation(s) for webhook logs.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Nursa Public Api V2 Webhooks](https://apis.io/asyncapis/nursa/nursa-public-api-v2-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [nursa-mcp.yml](https://apis.io/servers/nursa/nursa-mcp-yml/)

#### Collections (13)

Postman and Open Collection request collections.

- [Nursa Public API V2 Clinicians API](https://apis.io/collections/nursa/open-nursa-clinicians-api/)
- [Nursa Public API V2 Downloads API](https://apis.io/collections/nursa/open-nursa-downloads-api/)
- [Nursa Public API V2 Facilities API](https://apis.io/collections/nursa/open-nursa-facilities-api/)
- [Nursa Public API V2 Facilities webhooks API](https://apis.io/collections/nursa/open-nursa-facilities-webhooks-api/)
- [Nursa Public API V2 Licenses API](https://apis.io/collections/nursa/open-nursa-licenses-api/)
- [Nursa Public API V2 Marketplace API](https://apis.io/collections/nursa/open-nursa-marketplace-api/)
- [Nursa Public API V2 Scheduled shifts API](https://apis.io/collections/nursa/open-nursa-scheduled-shifts-api/)
- [Nursa Public API V2 Shift reports API](https://apis.io/collections/nursa/open-nursa-shift-reports-api/)
- [Nursa Public API V2 Shift requests API](https://apis.io/collections/nursa/open-nursa-shift-requests-api/)
- [Nursa Public API V2 Shifts API](https://apis.io/collections/nursa/open-nursa-shifts-api/)
- [Nursa Public API V2 Support API](https://apis.io/collections/nursa/open-nursa-support-api/)
- [Nursa Public API V2 User webhooks API](https://apis.io/collections/nursa/open-nursa-user-webhooks-api/)
- [Nursa Public API V2 Webhook logs API](https://apis.io/collections/nursa/open-nursa-webhook-logs-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Nursa Public Api V2 Overlay](https://apis.io/overlays/nursa/nursa-public-api-v2-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Nursa Authentication](https://apis.io/security/nursa/nursa-authentication/)
- [Nursa Domain Security](https://apis.io/security/nursa/nursa-domain-security/)
- [Nursa Trust Center](https://apis.io/security/nursa/nursa-trust-center/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Nursa Scopes](https://apis.io/scopes/nursa/nursa-scopes/)

#### Rate limits (1)

Published request limits.

- [Nursa Rate Limits](https://apis.io/rate-limits/nursa/nursa-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://nursa.com/)
- [Documentation](https://docs.nursa.com/)
- [Getting started](https://docs.nursa.com/docs/Integration%20Guideline/First%20steps/)
- [API reference](https://docs.nursa.com/pages/api/nursa-public-api-v-2)
- [Pricing](https://nursa.com/facility/pricing)
- [Sign up](https://nursa.com/signup/facility)
- [Support](https://nursa.com/support)
- [Blog](https://nursa.com/blog)
- [Compliance](https://trust.nursa.com/)
- [Terms of service](https://nursa.com/terms)
- [Privacy policy](https://nursa.com/privacy-policy)
- [GitHub](https://github.com/nursa-com)

## API Evangelist rating

**Kin Score 55.7** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 61.7 |
| Contract governance | 4.5 |
| Operational transparency | 31.6 |
| Developer ergonomics | 66.1 |
| Access clarity | 60.5 |

**Agent readiness 36.0** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 36.7** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/nursa/](https://apis.io/providers/nursa/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Nursa: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/nursa/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
