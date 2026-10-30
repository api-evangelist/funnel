# Funnel

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

Funnel (funnel.io) is a marketing intelligence and marketing data hub that helps agencies and brands become more data-driven. It connects to hundreds of advertising, analytics, CRM, and social data platforms, then automatically collects, normalizes, and transforms that marketing data into a single, business-ready model. Funnel exports the harmonized data to cloud data warehouses (BigQuery, Snowflake), Google Cloud Storage, BI and visualization tools, and back to ad platforms, and layers on advanced marketing measurement (Marketing Mix Modeling and Multi-Touch Attribution) plus dashboards and reporting. Programmatic configuration is exposed through the Funnel Control Plane API, consumed via an official Terraform provider and an OAuth 2.0 client-credentials (Auth0) system-user flow, with regional US and EU data residency. Funnel also ships a first-party hosted MCP server (Funnel MCP) that exposes six read-only tools over OAuth to Claude, ChatGPT, Cursor and other MCP clients, plus a documented inbound file-import webhook API. Funnel publishes no OpenAPI or AsyncAPI for any of the three surfaces.

**Areas:** Company · Marketing · Marketing Intelligence · Marketing Data · Analytics · Advertising · Data Integration · ETL · Data Warehouse · Attribution · Reporting · Business Intelligence

## Funnel on APIs.io

Everything profiled for Funnel is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/funnel/](https://apis.io/providers/funnel/)** — or go straight to one of the APIs or properties below.

### APIs (3)

- [Funnel Control Plane API](https://apis.io/apis/funnel/funnel-control-plane-api/) — The Funnel Control Plane API provides configuration-management operations for a Funnel subscription — workspaces, data sources, custom dimensions and…
- [Funnel MCP](https://apis.io/apis/funnel/funnel-mcp/) — Funnel MCP is Funnel's first-party hosted, remote Model Context Protocol server. It exposes a Funnel workspace's harmonized cross-channel marketing data,…
- [Funnel File Import Webhook API](https://apis.io/apis/funnel/funnel-file-import-webhook-api/) — The Funnel File Import Webhook API is a documented inbound HTTP endpoint that lets a customer hand Funnel links to data files for ingestion into a File…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Funnel Webhooks](https://apis.io/asyncapis/funnel/funnel-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Funnel MCP](https://apis.io/servers/funnel/funnel-mcp/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Funnel Authentication](https://apis.io/security/funnel/funnel-authentication/)
- [Funnel Domain Security](https://apis.io/security/funnel/funnel-domain-security/)
- [Funnel Trust Center](https://apis.io/security/funnel/funnel-trust-center/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Funnel Scopes](https://apis.io/scopes/funnel/funnel-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Funnel Plans Pricing](https://apis.io/plans/funnel/funnel-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Funnel Rate Limits](https://apis.io/rate-limits/funnel/funnel-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://funnel.io/)
- [Documentation](https://help.funnel.io/en/)
- [API reference](https://registry.terraform.io/providers/funnel-io/funnel/latest/docs)
- [Pricing](https://funnel.io/pricing)
- [Sign up](https://app.funnel.io/)
- [Support](https://help.funnel.io/en/)
- [Blog](https://funnel.io/blog)
- [Compliance](https://funnel.io/funnel-information-security-overview)
- [Terms of service](https://funnel.io/general-terms-and-conditions)
- [Privacy policy](https://funnel.io/privacy)
- [GitHub](https://github.com/funnel-io)
- [Status](https://status.funnel.io)

## API Evangelist rating

**Kin Score 58.6** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 80.0 |
| Contract quality | 44.2 |
| Contract governance | 18.2 |
| Operational transparency | 42.1 |
| Developer ergonomics | 49.4 |
| Access clarity | 92.1 |

**Agent readiness 48.7** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 39.2** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/funnel/](https://apis.io/providers/funnel/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Funnel: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/funnel/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
