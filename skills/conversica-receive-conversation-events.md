---
name: Receive Conversica message and lead-update webhooks
description: Implement the two customer-hosted endpoints Conversica posts to — Message objects and partial Lead Update objects — so conversation state flows back into the customer system correctly.
api: openapi/conversica-integrations-api-openapi.yml
operations:
  - receiveMessage
  - receiveLeadUpdate
generated: '2026-08-01'
method: generated
source: openapi/conversica-integrations-api-openapi.yml
---

# Receive Conversica message and lead-update webhooks

Conversica exposes **no read API**. The only way to learn what an Assistant said, what the
lead replied, or whether a lead went hot is to host the two endpoints Conversica posts to.
Build them before go-live.

## What you must provide

Two publicly reachable HTTPS endpoints that accept `POST` with a JSON body, protected by
HTTP Basic authentication. **Both endpoints share one username/password pair** that you
supply to Conversica; usernames have a five-character minimum. Use one consistent URL shape,
e.g. `https://api.yourcompany.com/conversica/message` and
`https://api.yourcompany.com/conversica/lead`.

## Steps

1. **Implement `receiveMessage`.** Schema:
   `openapi/conversica-integrations-api-openapi.yml#/components/schemas/Message`.
   All keys are required: `apiVersion`, `id`, `clientId`, `action`, `date`, `subject`,
   `body`. `action` is `sent` (Assistant → lead) or `received` (lead → Assistant). Append
   these to the conversation history on the record whose customer-system id equals `id`.
2. **Implement `receiveLeadUpdate`.** Schema:
   `openapi/conversica-integrations-api-openapi.yml#/components/schemas/LeadUpdate`.
   **This is a partial payload.** Only `apiVersion`, `id` and `clientId` are guaranteed;
   every other key appears only when its value changed. **Merge — never replace.** A missing
   key means "unchanged", not "cleared". This is the single most common integration bug on
   this API.
3. **Join on `id`.** It is *your* identifier (the Source ID you sent on `postLead`), not a
   Conversica identifier. Conversica never issues an id of its own through this API.
   `clientId` is the tenant key for provider-style integrations serving several Conversica
   customers.
4. **Act on the escalation flags.** `hotLead: true` means the person is ready for a human —
   route it to the rep in `repId` immediately. `actionRequired: true` means the Assistant has
   **stopped messaging** pending human review. `leadAtRisk: true` means an interested lead has
   not yet been contacted by the salesperson.
5. **Honour the discovered contact data.** `discoveredPhone1/2` and `discoveredEmail1/2` are
   new contact details the Assistant extracted from replies. Store them; do not overwrite
   verified primary contact fields blindly.
6. **Honour consent changes.** `doNotEmail`, `smsOptIn` and `smsOptOut` on a Lead Update are
   consent state changes that arrived from the person themselves. Propagate them into your
   suppression lists — they are legally significant, not cosmetic.
7. **Map the state fields through the published vocabulary.** `leadStatus`,
   `conversationStage` and `conversationStatus` are controlled values; the full list with
   definitions is in `vocabulary/conversica-conversation-vocabulary.yml`. Do not invent your
   own mapping — statuses such as `Bounced`, `Unsubscribed`, `Marked Spam` and
   `No Longer at Company` each mean something specific.
8. **Reply fast with 2xx.** Conversica publishes no retry, backoff or signing contract, so
   treat delivery as at-most-once from your side: acknowledge quickly and process
   asynchronously rather than doing slow work inline.

## Website chat leads (optional)

If you use Conversica Website Chat, add a third endpoint for `receiveChatLead`
(`.../conversica/create`). It receives `firstName`, `lastName`, `email` and
`conversationHistory` — the full transcript — and **must** reply with a JSON object carrying
`status` and `message`, where `status` is the HTTP code as a *string* (`"200"`, `"400"`,
`"401"`, `"500"`). See `examples/conversica-chat-webhook-ack.json`.

## Security notes

- These endpoints receive personal data and full conversation transcripts. Terminate TLS
  strictly, verify Basic credentials on every request, and rate-limit the endpoints yourself.
- Conversica publishes no webhook signature or shared-secret HMAC, so Basic auth plus source
  IP hygiene is the whole of the authentication story. Rotate the credential on your own
  schedule.
- `my.conversica.com` and `integrations-api.conversica.com` are in scope of Conversica's
  responsible disclosure policy — report issues to `security@conversica.com`.

## Reference

- Webhook catalogue: `asyncapi/conversica-webhooks.yml`
- Conventions: `conventions/conversica-conventions.yml`
- Examples: `examples/conversica-message-sent.json`,
  `examples/conversica-lead-update-engagement.json`,
  `examples/conversica-lead-update-stage.json`
