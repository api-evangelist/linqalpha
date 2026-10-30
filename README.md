# LinqAlpha

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

LinqAlpha is a domain-specialized multi-agent AI platform for institutional investment research, serving hedge funds, asset managers and investment banks. It runs retrieval-augmented, agentic analysis across SEC filings, earnings-call transcripts, IR materials, news and macroeconomic data covering 57,600+ companies in 120+ countries and 30+ languages, returning answers with inline citations that resolve to the exact source chunks. The LinqAlpha API exposes that engine over a REST surface at api.linqalpha.com — streaming analytics and chat over Server-Sent Events, document search, natural-language-to-SQL, scheduled research briefings, an RMS/Vault ingestion path for a firm's own internal documents, and a source-grounding judge that scores how well each claim in an answer is backed by the sources it cited. LinqAlpha also publishes a first-party remote MCP server exposing 22 financial-data tools to AI assistants.

**Areas:** Company · Financial Services · Investment Research · Artificial Intelligence · Agents · Market Data · Stocks · Economic Data · SEC Filings · RAG · MCP

## LinqAlpha on APIs.io

Everything profiled for LinqAlpha is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/linqalpha/](https://apis.io/providers/linqalpha/)** — or go straight to one of the APIs or properties below.

### APIs (10)

- [LinqAlpha Briefing API](https://apis.io/apis/linqalpha/linqalpha-briefing-api/) — Briefing Agent — automated market briefings with scheduling and delivery
- [LinqAlpha Connectors API](https://apis.io/apis/linqalpha/linqalpha-connectors-api/) — Customer Connectors — customer-owned MCP connector management
- [LinqAlpha Data API](https://apis.io/apis/linqalpha/linqalpha-data-api/) — Data retrieval and mapping
- [LinqAlpha Feedback API](https://apis.io/apis/linqalpha/linqalpha-feedback-api/) — Conversation feedback
- [LinqAlpha MCP API](https://apis.io/apis/linqalpha/linqalpha-mcp-api/) — LinqAlpha MCP — Financial data tools for AI assistants via Model Context Protocol
- [LinqAlpha RMS API](https://apis.io/apis/linqalpha/linqalpha-rms-api/) — Research Management System
- [LinqAlpha Search API](https://apis.io/apis/linqalpha/linqalpha-search-api/) — Search and generate responses
- [LinqAlpha Source Management API](https://apis.io/apis/linqalpha/linqalpha-source-management-api/) — Source batch and file management
- [LinqAlpha Status API](https://apis.io/apis/linqalpha/linqalpha-status-api/) — Sync status — check organization, document, and container sync progress
- [LinqAlpha Vault API](https://apis.io/apis/linqalpha/linqalpha-vault-api/) — The Vault API from LinqAlpha — 3 operation(s) for vault.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [linqalpha](https://apis.io/servers/linqalpha/linqalpha/)

#### Collections (10)

Postman and Open Collection request collections.

- [LinqAlpha Briefing API](https://apis.io/collections/linqalpha/open-linqalpha-briefing-api/)
- [LinqAlpha Briefing Connectors API](https://apis.io/collections/linqalpha/open-linqalpha-connectors-api/)
- [LinqAlpha Briefing Data API](https://apis.io/collections/linqalpha/open-linqalpha-data-api/)
- [LinqAlpha Briefing Feedback API](https://apis.io/collections/linqalpha/open-linqalpha-feedback-api/)
- [LinqAlpha Briefing MCP API](https://apis.io/collections/linqalpha/open-linqalpha-mcp-api/)
- [LinqAlpha Briefing RMS API](https://apis.io/collections/linqalpha/open-linqalpha-rms-api/)
- [LinqAlpha Briefing Search API](https://apis.io/collections/linqalpha/open-linqalpha-search-api/)
- [LinqAlpha Briefing Source Management API](https://apis.io/collections/linqalpha/open-linqalpha-source-management-api/)
- [LinqAlpha Briefing Status API](https://apis.io/collections/linqalpha/open-linqalpha-status-api/)
- [LinqAlpha Briefing Vault API](https://apis.io/collections/linqalpha/open-linqalpha-vault-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Linqalpha Api Overlay](https://apis.io/overlays/linqalpha/linqalpha-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Linqalpha Authentication](https://apis.io/security/linqalpha/linqalpha-authentication/)
- [Linqalpha Domain Security](https://apis.io/security/linqalpha/linqalpha-domain-security/)
- [Linqalpha Trust Center](https://apis.io/security/linqalpha/linqalpha-trust-center/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Linqalpha Scopes](https://apis.io/scopes/linqalpha/linqalpha-scopes/)

#### Rate limits (1)

Published request limits.

- [Linqalpha Rate Limits](https://apis.io/rate-limits/linqalpha/linqalpha-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://linqalpha.com/)
- [Documentation](https://docs.linqalpha.com/quickstart)
- [Getting started](https://docs.linqalpha.com/quickstart)
- [API reference](https://docs.linqalpha.com/api-reference/basic/analytics-sse)
- [Sign up](https://chat.linqalpha.com/analytics)
- [Support](https://linqalpha.com/contact-sales)
- [Blog](https://linqalpha.com/blog)
- [Compliance](https://linqalpha.com/security)
- [Terms of service](https://linqalpha.com/terms-of-use)
- [Privacy policy](https://linqalpha.com/privacy-policy)
- [Status](https://status.linqalpha.com/)

## API Evangelist rating

**Kin Score 47.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 51.6 |
| Contract governance | 18.2 |
| Operational transparency | 36.8 |
| Developer ergonomics | 58.9 |
| Access clarity | 36.8 |

**Agent readiness 55.5** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
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
| Operation Distinctiveness | distinct |

**Regulatory alignment 34.5** — Securities & Market Data

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/linqalpha/](https://apis.io/providers/linqalpha/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for LinqAlpha: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/linqalpha/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
