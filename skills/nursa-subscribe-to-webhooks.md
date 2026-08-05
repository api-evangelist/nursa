---
name: Subscribe to and verify Nursa webhooks
description: Register Nursa facility and user webhooks, verify the Nursa-Signature HMAC, handle the retry schedule, and audit delivery from the notification logs.
api: openapi/nursa-public-api-v2-openapi.yml
operations:
  - FacilitiesWebhooksController_createOne
  - FacilitiesWebhooksController_getAll
  - FacilitiesWebhooksController_updateOne
  - FacilitiesWebhooksController_deleteOne
  - UserWebhooksController_createOneUserWebhook
  - UserWebhooksController_getAllUserWebhooks
  - UserWebhooksController_updateOneUserWebhook
  - UserWebhooksController_deleteOneUserWebhook
  - WebhookLogsController_searchLogs
generated: '2026-08-04'
method: generated
source: openapi/nursa-public-api-v2-openapi.yml + https://docs.nursa.com/docs/Integration%20Guideline/Webhooks%20configuration/
---

# Subscribe to and verify Nursa webhooks

Nursa's docs tell you plainly that polling is the wrong shape: "Constantly requesting the API may be
sub-optimal." With a 100 req/min ceiling and no rate-limit headers, webhooks are the only sane way
to stay in sync.

## Two subscription kinds

| Kind | Create | Events it carries |
|---|---|---|
| Facility | `FacilitiesWebhooksController_createOne` (`POST /api/v2/public/webhooks/facility`) | the 10 `shift.*` events |
| User | `UserWebhooksController_createOneUserWebhook` (`POST /api/v2/public/webhooks/user`) | the 4 `facility.*` events |

Facility webhooks take an optional `facilities[]` array to scope delivery to specific facility ids.
Both take `url`, `events[]` and up to two `secrets`. If you omit `secrets`, Nursa generates one —
capture it, because you need it to verify.

The full event list with payload fields is in `asyncapi/nursa-public-api-v2-webhooks.yml`.

## Verify every delivery

Header:

```
Nursa-Signature: t=1492774577, v1=<hex>, v1=<hex>
```

1. Split on `,`, then on `=`. `t` is unix seconds; each `v1` is a candidate signature.
2. Build `<t> + "." + <raw request body>`. Use the **raw** body — re-serializing the JSON changes
   the bytes and breaks the HMAC.
3. HMAC-SHA256 with your webhook secret, hex encode.
4. Compare against every `v1` with a constant-time comparison. One match is enough — two secrets
   can be live at once so you can rotate with no downtime.
5. Check `t` against your own tolerance and reject stale deliveries. Nursa publishes no maximum
   age, so pick one (five minutes is conventional) and enforce it yourself.

Nursa publishes verification snippets in Python, JavaScript and Java at the configuration page.

## Respond correctly

Return a 2xx as soon as you have durably queued the event. Any status `>= 400` triggers a retry, and
there are only three: at a minimum of **5 minutes**, then **30 minutes**, then **60 minutes**. After
the third failure the event is dropped — there is no dead-letter queue and no manual replay
endpoint. If your consumer is down for more than about 95 minutes, you have permanently lost those
events and must reconcile by reading `MarketplaceController_getMarketplaceShifts`,
`ShiftRequestsController_getShiftRequests` and
`MarketplaceShiftReportsController_getAllShiftShiftReports` directly.

## Be idempotent on the receiving end

Retries mean you will see the same event more than once, and the envelope carries no event id — only
`{data, eventType}`. De-duplicate on the natural key instead: `eventType` plus `shiftId` plus the
`at` timestamp.

## Audit what was delivered

`WebhookLogsController_searchLogs` — `GET /api/v2/public/webhooks/logs`. Filter by `eventType`,
`facilityId`, `startDate`, `endDate`. The `retries` property shows every attempt, which is how you
prove whether a missing event was never sent or never accepted.

## Watch the semantics, not just the names

- `shift.request.cancelled` fires when a facility accepts a different clinician — every losing
  request is cancelled. It is not necessarily a clinician withdrawing.
- `shift.scheduled` carries `scheduledBy.autoScheduleContext.issued` — `true` means Nursa's auto
  schedule placed the clinician, not a human at the facility.
- `shift.report.accepted-automatically` means the 48-hour review window expired with no facility
  report. Treat it as a missed review, not as an approval.
