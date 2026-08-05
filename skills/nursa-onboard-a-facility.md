---
name: Onboard a facility onto the Nursa API
description: Get an integrating application from zero to a connected facility it can post shifts for — sign-up, app registration, token, facility connection or creation.
api: openapi/nursa-public-api-v2-openapi.yml
operations:
  - FacilitiesController_requestConnection
  - FacilitiesController_requestFacilityCreation
  - FacilitiesController_searchFacilities
  - FacilitiesController_getAll
  - FacilitiesController_getOneById
  - FacilitiesController_getAllSpecialties
  - SupportFacilitiesController_associateUsersToFacility
generated: '2026-08-04'
method: generated
source: openapi/nursa-public-api-v2-openapi.yml + https://docs.nursa.com/docs/Integration%20Guideline/Facility%20Onboarding/
---

# Onboard a facility onto the Nursa API

Nothing in the Nursa API works until the authenticated user is **connected to a facility**. Writes
against an unconnected facility fail as `404 Facility not found`, which reads like a bad id and is
actually a permission problem. This is the first thing to get right.

## 0. Pre-API steps you cannot skip

These happen in Nursa's UI, not over the API:

1. Sign up as a Facility User — sandbox at `https://nursa-sandbox.web.app/`, production at
   `https://nursa.com/signup/facility`. Password: 8+ characters and at least 3 of lowercase,
   uppercase, digits, specials.
2. Register your application in the Developer Portal —
   `https://developers.sandbox.nursa.com/` or `https://developers.prod.nursa.com/` — to get a
   Client ID and Client Secret.
3. Get a token from `https://auth.nursa.com/oidc/oauth/token`
   (sandbox: `https://auth.sandbox.nursa.com/oidc/oauth/token`).

**Machine-to-machine is gated.** Nursa enables the client credentials grant on an application only
on explicit request, because every action in Nursa has to be attributable to a user with access to
the target facility. Ask for it before you design around it. Build and test in sandbox first —
that is the documented path, and production access is a conversation with Nursa's integrations team
(`josh.bear@nursa.com`).

## 1. Find the facility

- `FacilitiesController_searchFacilities` — `GET /api/v2/public/facilities/search` — search the
  full Nursa directory.
- `FacilitiesController_getAll` — `GET /api/v2/public/facilities` — list only what you are already
  connected to. Note this one paginates with `page` + `limit` while the search operations use
  `limit` + `offset`; see `conventions/nursa-conventions.yml`.
- `FacilitiesController_getOneById` — `GET /api/v2/public/facilities/{facilityId}` — facility ids
  look like `NUR-123456`.

## 2a. The facility exists — request a connection

`FacilitiesController_requestConnection` — `POST /api/v2/public/facilities/connection`.

This is asynchronous and human-approved. Do not block on it. Subscribe a **user** webhook to
`facility.user-connection.accepted` and `facility.user-connection.rejected` and drive your
onboarding state machine off those.

`400 Facility connection request already exists` means a request is pending, not that something
broke. Treat it as success-in-progress.

## 2b. The facility does not exist — request creation

`FacilitiesController_requestFacilityCreation` — `POST /api/v2/public/facilities/request`.

Also asynchronous. Listen for `facility.creation.accepted` (payload carries the new `facilityId`)
or `facility.creation.rejected` (payload carries only `facilityName`, so key your pending record on
the name).

## 3. Confirm you can act

Once connected:

- `FacilitiesController_getAllSpecialties` — the facility specialty taxonomy.
- `LicensesController_getAll` — valid `licenseType` codes for posting.
- `FacilitiesController_getWorkHistory` — which clinicians have worked here before.

Then move to the post-and-fill skill.

## Additional users

`SupportFacilitiesController_associateUsersToFacility` —
`PUT /api/v2/support/facilities/user/associate` — associates users with a facility. Note it sits
outside the `/api/v2/public` namespace, is the only untagged operation in the spec, and is a support
path — confirm with Nursa before wiring it into a normal onboarding flow.

## Common stalls

- `404 Facility not found` on any write — you are not connected. Go to 2a.
- No associated user — a facility needs at least one user associated before shifts can be posted.
- Auto-schedule option not enabled — the facility does not have the auto-schedule feature you asked
  for on the shift; pass `autoScheduleSettings: []` to opt out.
