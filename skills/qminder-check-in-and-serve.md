---
name: Check a visitor into a queue and serve them
description: Add a visitor to a Qminder line as a ticket, call them for service, then mark them served.
api: openapi/qminder-openapi-original.json
operations: [create-ticket, calling-a-ticket, marking-a-ticket-as-served]
---

# Check a visitor into a queue and serve them

Use the Qminder REST API (`https://api.qminder.com`). Authenticate every request with the
`X-Qminder-REST-API-Key: <key>` header (HTTPS required). Respect the rate limit of 5 requests/second
per key and honor `X-Ratelimit-Remaining`; on HTTP 429 back off.

## Steps

1. **Create the ticket** — `create-ticket` (`POST /tickets`). Provide the target `lineId` and any
   visitor details (name, contact, input-field values). The response returns the new ticket id.
2. **Call the ticket** — `calling-a-ticket` (`POST /v1/tickets/{id}/call`) when a desk is ready to
   serve the visitor. Optionally pass the serving user/desk.
3. **Mark served** — `marking-a-ticket-as-served` (`POST /v1/tickets/{id}/markserved`) once the
   visit is complete.

## Notes
- If the visitor leaves, use `marking-a-ticket-as-no-show` instead of markserved.
- Errors return a JSON body `{statusCode, message, developerMessage}` (not RFC 9457). A 400 is a
  validation error; 404 means the line/ticket id does not exist.
- To react to state changes instead of polling, register a webhook (see the manage-webhooks skill)
  for `ticket_created`, `ticket_called`, and `ticket_served`.
