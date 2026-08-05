---
name: Reconcile a Nursa shift report
description: Review, accept, reject or cancel the hours a clinician reported for a completed Nursa shift, before the 48-hour auto-accept deadline.
api: openapi/nursa-public-api-v2-openapi.yml
operations:
  - MarketplaceShiftReportsController_getAllShiftShiftReports
  - MarketplaceShiftReportsController_acceptShiftReport
  - MarketplaceShiftReportsController_rejectShiftReport
  - MarketplaceShiftReportsController_cancelShiftReport
  - DownloadController_clinicianProfile
generated: '2026-08-04'
method: generated
source: openapi/nursa-public-api-v2-openapi.yml + https://docs.nursa.com/docs/Integration%20Guideline/Shift%20Reports/
---

# Reconcile a Nursa shift report

After a clinician works a shift they submit a shift report with their hours. The facility reviews it
against its own records. This is the step that determines what the facility pays, so the clock
matters.

## The deadline is the whole point

If the facility does not submit its own report within **48 hours** of the clinician's submission,
Nursa accepts the clinician's report automatically and fires
`shift.report.accepted-automatically`. Silence is agreement. Any integration that surfaces shift
reports to a human must carry that deadline into the queue, or the facility loses its review window
by default.

## 1. Know a report exists

Subscribe to `shift.report.created` (see `asyncapi/nursa-public-api-v2-webhooks.yml`) rather than
polling. Then read it:

- `MarketplaceShiftReportsController_getAllShiftShiftReports` —
  `GET /api/v2/public/marketplace/shifts/reports/{shiftId}`.

`reportType` distinguishes the clinician report (`Nurse`) from the facility report (`Facility`) and
the system's own (`Admin`).

## 2. Accept, reject or cancel

- Agree with the hours: `MarketplaceShiftReportsController_acceptShiftReport` —
  `POST /api/v2/public/marketplace/shifts/reports/{shiftId}/accept`. Fires
  `shift.report.accepted`.
- Disagree: `MarketplaceShiftReportsController_rejectShiftReport` —
  `POST /api/v2/public/marketplace/shifts/reports/{shiftId}/reject`. Fires
  `shift.report.rejected`. A rejection is a discrepancy between the two reports, so expect Nursa to
  get involved.
- Report an incident instead: `MarketplaceShiftReportsController_cancelShiftReport` —
  `POST /api/v2/public/marketplace/shifts/reports/{shiftId}/cancel`. `cancellationComment` is a
  closed set: `No Call No Show`, `Excused Emergency`, `Refused to Work`,
  `No Longer Want to Work With This Clinician`.

A review rating, where accepted, is an integer 1-5 (`rate must not be greater than 5`,
`rate must not be less than 1`, `rate must be an integer number`).

## 3. Errors you will actually hit

From `errors/nursa-problem-types.yml`:

- `400 Shift must be completed to submit a shift report` — you are early.
- `400 Clinician shift report was not submitted` — there is nothing to reconcile yet; wait for
  `shift.report.created`.
- `400 Facility shift report already submitted` — this is your idempotency signal in disguise.
  There is no `Idempotency-Key` on this API, so treat this 400 as "already applied", not as a
  failure to retry.
- `400 Clinician is not scheduled for the shift.` — wrong shift or wrong clinician.

## 4. Evidence for the file

`DownloadController_clinicianProfile` —
`GET /api/v2/public/downloads/clinician/{clinicianId}/shift/{shiftId}` — returns a PDF snapshot of
the clinician's profile as of that shift. If it 404s with `Clinician profile url is not available
yet, try again later.` the snapshot is still generating; back off and retry rather than treating it
as missing.
