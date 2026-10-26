# Conversica

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

Conversica is an AI conversation automation company — "The Conversation Company" — whose
Revenue Digital Assistants and AI Agents hold two-way, natural-language conversations with
leads and customers over email, SMS and website chat. Founded in 2007 as AutoFerret.com and
renamed Conversica in 2014, the San Mateo company serves automotive, sports and
entertainment, hospitality, higher education and enterprise teams.

- Website: https://www.conversica.com
- Help centre: https://help.conversica.com
- API docs: https://help.conversica.com/hc/en-us/sections/360012154451-Conversica-API
- Trust centre: https://trust.conversica.com

## The API

**Conversica Integrations API** — `https://integrations-api.conversica.com`

A single JSON-over-HTTPS ingest endpoint (`POST /json/`) protected by HTTP Basic
authentication, plus three provider-initiated webhooks — Message, Lead Update and Website
Chat lead creation — delivered to endpoints the customer hosts. There is **no read or list
operation**: conversation state comes back only through the webhooks. The API version
travels in the JSON body as `apiVersion` (currently `7.2`), not in the URL.

Access is gated — credentials are issued by a Conversica technical account manager and
applications must be approved by Conversica before they are enabled.

Conversica publishes no machine-readable specification. `openapi/` holds an OpenAPI 3.1
document transcribed by the API Evangelist enrichment pipeline from Conversica's published
API Integration Manual and Website Chat webhook article; every path, field, type,
requiredness, status code and example in it comes from those documents.

## What is here

| Directory | Contents |
|---|---|
| `openapi/` | Captured OpenAPI 3.1 — 1 operation, 3 webhooks, 5 schemas |
| `overlays/` | API Evangelist annotations and recorded contract gaps |
| `asyncapi/` | Webhook catalogue (no AsyncAPI is published) |
| `authentication/` | Basic auth profile, plus the documented outbound OAuth 2.0 path |
| `conventions/` | Auth, versioning, partial updates, error envelope, absent idempotency/pagination |
| `errors/` | Published status codes and the `status`/`message` ack envelope |
| `vocabulary/` | Lead Status values and every Conversation Stage/Status with definitions |
| `data-model/` | Lead / Message / LeadUpdate / ChatLead entity graph |
| `examples/` | Eight request and response payloads published verbatim by Conversica |
| `sandbox/` | The Test Lead Token mechanism |
| `lifecycle/` | Versioning, the 30-day change-notice term, absent SLA and status page |
| `conformance/` | Cross-cutting standards posture |
| `security/` | Domain security probe, responsible disclosure policy, trust centre |
| `components/` | Website Chat JavaScript widget and packaged CRM connectors |
| `packages/` | Empty — Conversica ships no first-party client libraries |
| `mcp/` | Candidate only — Conversica publishes no MCP server |
| `well-known/` | Probe record — no `/.well-known/` document is served on any host |
| `agentic-access/` | Recommended execution contract for agent use |
| `skills/` | Packaged agent skills for posting leads and receiving conversation events |
| `llms/` | Generated `llms.txt` (Conversica serves none) |

## Notable gaps

No SDKs, no CLI, no MCP server, no A2A agent card, no `/.well-known/` documents, no
`security.txt` despite a published responsible disclosure policy, no idempotency contract,
no published rate limits, no dated changelog, no SLA, and no live status page — the
`status.conversica.com` host does not resolve and the underlying Statuspage instance is
inactive.
