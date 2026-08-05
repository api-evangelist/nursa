---
name: Post and fill a per diem shift on Nursa
description: Quote, post, review requests for and schedule a clinician on a Nursa per diem shift, from a facility scheduling system.
api: openapi/nursa-public-api-v2-openapi.yml
operations:
  - LicensesController_getAll
  - FacilitiesController_searchFacilities
  - MarketplaceController_quoteShift
  - MarketplaceController_createShiftFromQuote
  - MarketplaceController_createShifts
  - ShiftRequestsController_getShiftRequests
  - CliniciansController_getDetails
  - ScheduleShiftsController_scheduleShift
generated: '2026-08-04'
method: generated
source: openapi/nursa-public-api-v2-openapi.yml + https://docs.nursa.com/
---

# Post and fill a per diem shift on Nursa

Use this when a facility needs a PRN clinician on the Nursa marketplace. Every step below is a real
operation in `openapi/nursa-public-api-v2-openapi.yml`.

## Before you start

- Base URL: `https://public-api.prod.nursa.com` (sandbox: `https://public-api.sandbox.nursa.com`).
- Auth: `Authorization: Bearer <JWT>` from `https://auth.nursa.com/oidc/oauth/token`. Cache the
  token — it lives 1 hour in production, and requesting a new one per call will get you rate
  limited. See `authentication/nursa-authentication.yml`.
- Scopes: `marketplace:write` to post, `marketplace:read` to read, `shift-requests:read` and
  `shift-requests:write` to work requests. The spec declares an empty scope array on every
  operation, so read `scopes/nursa-scopes.yml`, not the spec, to know what to request.
- Rate limit: 100 requests/minute. There are no rate-limit headers — budget client-side.

## 1. Resolve the license type and the facility

- `LicensesController_getAll` — `GET /api/v2/public/licenses`. Returns the valid `licenseType`
  codes. A shift without a valid one is rejected.
- `FacilitiesController_searchFacilities` — `GET /api/v2/public/facilities/search` — or
  `FacilitiesController_getAll` to list the facilities you are already connected to.

If the facility comes back "facility not found" on a later write, you are not connected to it: call
`FacilitiesController_requestConnection` (`POST /api/v2/public/facilities/connection`) and wait for
the `facility.user-connection.accepted` webhook.

## 2. Price the shift, then post it

Preferred, two-step:

1. `MarketplaceController_quoteShift` — `POST /api/v2/public/marketplace/shifts/quote`. Price is
   computed from `from`, `to` and optional `breakTime`.
2. `MarketplaceController_createShiftFromQuote` — `POST /api/v2/public/marketplace/shifts/quote/{quoteId}/shift`.

Use this pair whenever you can. A `quoteId` is single-use, which makes it the ONLY replay-safe
handle in this API — there is no `Idempotency-Key` header anywhere (see
`conventions/nursa-conventions.yml`).

Bulk alternative:

- `MarketplaceController_createShifts` — `POST /api/v2/public/marketplace/shifts/batch`. Body:
  `facilityId` plus a `shifts[]` array of `{licenseType, description, from, to, breakTime?,
  autoScheduleSettings?}`.

Rules that will bite you:

- `to` must be less than 24 hours after `from`.
- The batch is transactional: one bad row reverts them all.
- **Do not blindly retry a timed-out batch.** With no idempotency key you cannot tell whether the
  first attempt landed, and every duplicated shift is a real financial commitment. Re-read with
  `MarketplaceController_getMarketplaceShifts` first, then decide.
- `autoScheduleSettings`: omit to inherit the facility's setting, pass specific options to narrow
  it, pass `null` or `[]` to opt out entirely.

## 3. Collect clinician requests

- `ShiftRequestsController_getShiftRequests` — `GET /api/v2/public/shift-requests`.

Do not poll this. Subscribe instead with `FacilitiesWebhooksController_createOne` and handle
`shift.request.created`. See `asyncapi/nursa-public-api-v2-webhooks.yml`.

Optional vetting before you commit:

- `CliniciansController_getDetails` — `GET /api/v2/public/clinicians/{clinicianId}/details`
- `CliniciansController_getDocuments` — credentials on file
- `FacilitiesController_getWorkHistory` — has this clinician worked here before

## 4. Schedule the clinician

- `ScheduleShiftsController_scheduleShift` — `POST /api/v2/public/scheduled-shifts`.

Accepting one request cancels every other request on that shift; you will receive one
`shift.scheduled` plus a `shift.request.cancelled` per losing request. Do not treat those
cancellations as clinician withdrawals.

To reject a specific request instead: `ShiftRequestsController_rejectShiftRequest`.

## 5. Handle the refusals

Errors are `{message, error, statusCode}` — not RFC 9457, and with no stable error code, so branch
on `message` text (`errors/nursa-problem-types.yml` lists the 23 documented messages).

- `400 Clinician is no longer available to be scheduled to this shift due to overlapping shifts.` —
  someone else took them. Go back to step 3.
- `400 Clinician is no longer available to be scheduled to this shift.` — same outcome.
- `403 Clinician is blocked` — do not retry with this clinician.
- `404 Facility not found` — you are not connected to the facility (step 1).
- `429` — rate limited. The body is **HTML**, not JSON; guard your parser on this path.

## Cancelling

`MarketplaceController_cancelShift` — `POST /api/v2/public/marketplace/shifts/cancel/{shiftId}`.
`cancelationReason` is a closed set: `Filled internally`, `Filled by other agency`,
`Talent not needed`, `No call no show`. To drop a scheduled clinician but keep the shift open, use
`ScheduleShiftsController_removeScheduledClinician`.
