# DeliSMS Self-Host Lab Baseline - 2026-07-08

## Status

Lab / foundation baseline achieved.

This is not Paid Client Release ready.
This is not DeliChow OS integrated.
This is not production-runtime proven beyond the lab smoke tests listed here.

## Repository

- Repo: jedidancel/delisms
- Branch: delichow-selfhost-lab
- Baseline tag: delisms-selfhost-baseline-2026-07-08

## Known-good deployed services

- API Cloud Run service: delisms-api
- API revision: delisms-api-00009-4kn
- API URL: https://delisms-api-924798676131.us-central1.run.app

- Web Cloud Run service: delisms-web
- Web revision: delisms-web-00005-hd4
- Web URL: https://delisms-web-924798676131.us-central1.run.app

## Verified runtime proof

The following lab path was verified:

DeliSMS Web UI
-> self-host DeliSMS API
-> internal /v1/events queue
-> Firebase Cloud Messaging
-> Android gateway phone
-> carrier SMS
-> webhook callback

## Passed checks

- Web UI login works.
- Web UI SMS send works.
- API accepts send request.
- Event queue auth works.
- Firebase FCM dispatch works.
- Android receives dispatch.
- Android sends carrier SMS.
- Delivery event returns to API.
- Webhook receives message.phone.delivered.
- Webhook includes signed Authorization Bearer JWT.
- Webhook includes x-event-type header.
- Phone API Key normal association works after Android pairing.
- Expired Firebase Web token issue was patched.
- Axiom telemetry 401 noise was patched.

## Important source fixes included

1. Web Firebase token refresh

The Web app now listens for Firebase ID token refresh events and refreshes the API Authorization header.

2. Axiom telemetry guard

The API no longer initializes Axiom exporters unless these env vars are configured:

- AXIOM_TOKEN
- AXIOM_DATASET_EVENTS
- AXIOM_DATASET_METRICS

3. Lab runtime fixes

- Self-host Android default API URL points to the lab API.
- Web Docker build uses pnpm install --ignore-scripts.
- Phone number authorization normalizes phone numbers before comparing.

## Known limitations

This baseline is still insufficient for a paid-client milestone.

Outstanding items:

- No DeliChow OS adapter yet.
- No tenant entitlement integration.
- No quota enforcement from DeliChow OS.
- No billing enforcement from DeliChow OS.
- No DeliChow SMS ledger integration.
- No merchant onboarding workflow.
- No fallback channel logic.
- No support playbook.
- No custom domain yet.
- Android package and branding are not finalized.
- Secret cleanup policy still needs a full repo audit.
- Webhook JWT issuer is still inherited from upstream and should be made configurable.
- Device health monitoring is not yet DeliChow-integrated.

## Licensing boundary

This gateway remains a separate AGPL service.

Do not copy AGPL gateway source code into the closed-source DeliChow SaaS repo.

DeliChow OS should integrate with this service only through HTTP APIs and webhooks.

## Next recommended milestone

Create a DeliChow SMS Gateway Adapter plan.

The adapter must live in the closed-source DeliChow SaaS repo and call this gateway externally.

The gateway remains responsible only for:

- SMS transport
- Android device pairing
- Firebase Cloud Messaging dispatch
- delivery / expiration events
- Android device status

DeliChow OS remains responsible for:

- tenant entitlement
- quota
- billing rules
- notification orchestration
- fallback logic
- audit ledger
- merchant dashboard settings
- customer consent rules
