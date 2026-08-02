---
name: Post a lead to a Conversica Assistant
description: Send a Lead object to the Conversica Platform so a Revenue Digital Assistant starts (or continues) an email/SMS conversation with that person, using HTTP Basic authentication over HTTPS.
api: openapi/conversica-integrations-api-openapi.yml
operations:
  - postLead
generated: '2026-08-01'
method: generated
source: openapi/conversica-integrations-api-openapi.yml
---

# Post a lead to a Conversica Assistant

Use this when a customer system (CRM, marketing automation platform, event tool) needs a
Conversica Assistant to begin conversing with a person.

**This operation causes an AI Assistant to email or text a named human being.** It is not a
benign record write. Do not call it speculatively, in a loop, or on unverified data.

## Before you start

- You need Basic-auth credentials for the Conversica endpoint. They are issued by a
  Conversica technical account manager — there is no self-serve signup, and the API terms
  require Conversica to approve your application before it is enabled.
- Confirm which `conversationId` the lead belongs to. That is the Conversation (the
  campaign-like list) that determines what the Assistant will say.
- If you are still in implementation, use the Test Lead Token instead of a real prospect —
  see `sandbox/conversica-sandbox.yml`. The token replaces the lead's **last name** and the
  default allowance is 10 test leads.

## Steps

1. **Build the Lead object.** Schema:
   `openapi/conversica-integrations-api-openapi.yml#/components/schemas/Lead`.
   Required keys: `apiVersion` (currently `"7.2"`), `id`, `conversationId`, `firstName`,
   `email`, `leadSource`, `repName`.
2. **Respect the type rules.** Every `datetime` value must be UTC in RFC 3339 form
   (`2019-05-10T05:57:44+00:00`). Every `boolean` must be literal `true`/`false`.
   Identifier-shaped fields (`id`, `clientId`, `repId`, `year`) are **strings**, not numbers.
3. **Carry consent forward.** If your source system knows the person opted out, set `optOut`
   (all channels) and/or `smsOptOut`. For SMS conversations you must supply `cellPhone` and
   `smsOptIn`. Never omit a known opt-out to get a conversation started.
4. **Add `clientId`** if this integration serves more than one Conversica customer.
5. **Call `postLead`** — `POST https://integrations-api.conversica.com/json/` with
   `Content-type: application/json` and HTTP Basic credentials.
6. **Read the status code.** `200` accepted, `400` the body could not be understood, `401`
   authentication failed. There is no response body schema and no application error code —
   the status code is the whole signal (`errors/conversica-problem-types.yml`).

## Automotive leads

For automotive Assistants the same operation accepts extra keys: `bdcRepName`,
`bdcRepEmail`, `serviceRepName`, `year`, `make`, `model`, `vin`, `appointmentStatus`,
`appointmentDate`, `leadSubStatus`. See `examples/conversica-lead-automotive.json`.

## Retries and duplicates

There is **no idempotency key**. If you retry a `postLead` after a timeout you may create a
second lead. Conversica de-duplicates server side against `id` and will surface repeats as
the `Duplicate (Internal)` / `Duplicate (CRM)` conversation statuses, but that is a
downstream cleanup, not a guarantee. Keep `id` stable per person and retry conservatively —
a fixed small number of attempts, never an unbounded retry loop.

## Stopping a conversation

Post the same `id` with `stopMessaging: true` to make the Assistant stop listening and stop
messaging. Use `skipToFollowup: true` to make it wait a few days before the next message.

## Examples

- `examples/conversica-lead-minimal.json`
- `examples/conversica-lead-automotive.json`
