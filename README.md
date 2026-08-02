# Conversica

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
