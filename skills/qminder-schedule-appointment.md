---
name: Schedule and check in an appointment
description: Create a scheduled Qminder appointment, check the visitor in on arrival, or mark a no-show.
api: openapi/qminder-openapi-original.json
operations: [create-appointment, checkin-appointment, mark-appointment-noshow]
---

# Schedule and check in an appointment

Authenticate with `X-Qminder-REST-API-Key`. Base URL `https://api.qminder.com`.

## Steps

1. **Create the appointment** — `create-appointment` (`POST /v1/appointments`) with the time slot,
   `lineId`, and the assigned user. To let Qminder pick an available user automatically, use
   `create-auto-assign-appointment` (`POST /v1/appointments/auto-assign`) instead.
2. **Check in on arrival** — `checkin-appointment` (`POST /v1/appointments/{ticketId}/checkin`) to
   move the visitor into the live queue.
3. **Handle a no-show** — `mark-appointment-noshow` (`POST /v1/appointments/{ticketId}/marknoshow`)
   if the visitor does not arrive. Use `cancel-appointment` (`POST /v1/appointments/{ticketId}/cancel`)
   to cancel ahead of time, or `edit-appointment` (`PATCH /v1/appointments/{ticketId}`) to change the
   slot or assignee (send only changed fields).

## Notes
- Appointments become tickets on check-in, so the ticket operations (call/serve) apply afterward.
- Errors use the `{statusCode, message, developerMessage}` envelope.
