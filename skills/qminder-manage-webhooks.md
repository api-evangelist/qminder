---
name: Register and manage webhook listeners
description: Register a Qminder webhook URL to receive ticket/line/location events, and remove it when done.
api: openapi/qminder-openapi-original.json
operations: [adding-a-webhook, removing-a-webhook]
---

# Register and manage webhook listeners

Authenticate with `X-Qminder-REST-API-Key`. Base URL `https://api.qminder.com`.

## Steps

1. **Add a webhook** — `adding-a-webhook` (`POST /v1/webhooks`) with the public HTTPS URL Qminder
   should POST to. The response returns the webhook id.
2. **Receive events** — Qminder POSTs `application/json` bodies shaped `{type, data}` for events:
   `location_created`, `location_changed`, `line_changed`, `ticket_created`, `ticket_called`,
   `ticket_recalled`, `ticket_changed`, `ticket_served`.
3. **Verify the signature** — validate the `X-Qminder-Signature` header (HMAC-SHA256) before trusting
   a payload.
4. **Remove a webhook** — `removing-a-webhook` (`DELETE /v1/webhooks/{id}`) when the listener is
   retired.

## Notes
- For real-time in-app updates, GraphQL subscriptions (e.g. `createdTickets`, `servedTickets`) are an
  alternative to webhooks.
- Errors use the `{statusCode, message, developerMessage}` envelope.
