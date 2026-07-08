# DeliSMS Web + Console UX Blueprint — 2026-07-08

Status: Lab / Spike / Foundation  
Paid Client Release ready: NO  
DeliChow OS integrated: NO  

## Product framing

DeliSMS is an external Android-based SMS transport gateway for DeliChow.

It is not the main DeliChow OS.
It is not a generic public SMS API SaaS.
It is not yet merchant-ready or paid-client-ready.

Preferred architecture:

DeliChow OS → Notification Orchestrator → SMS Gateway Adapter → external DeliSMS gateway → FCM → tenant Android phone → carrier SMS → webhook back to DeliChow

## Primary UX goal

The web console must answer these questions quickly:

- Is the Android phone connected?
- Is the gateway able to dispatch through FCM?
- Was the SMS sent by the phone?
- Was the webhook received?
- What failed?
- What should the operator or merchant do next?

## User roles

### 1. Public visitor

Needs:
- Understand what DeliSMS is
- Understand it is tied to DeliChow SMS gateway operations
- See safe, limited claims
- Avoid upstream httpSMS branding confusion

### 2. DeliChow operator/admin

Needs:
- See device health
- Run smoke tests
- Verify webhook delivery
- Diagnose failed SMS events
- Support tenant onboarding

### 3. Merchant/tenant owner

Needs:
- Pair Android phone
- Understand phone responsibility
- See connection status
- Send test SMS
- Know when the phone is offline or misconfigured

### 4. Developer/integrator

Needs:
- API key management
- Webhook setup
- Event payload references
- Safe integration guidance

## Public homepage direction

The homepage should position DeliSMS as:

“External Android SMS Gateway for DeliChow tenant messaging.”

Avoid:
- public SMS SaaS claims
- cheap pricing sections
- production-ready claims
- reliability guarantees
- enterprise-grade language without proof
- upstream httpSMS branding

Required homepage sections:

1. Hero
   - DeliSMS name
   - External Android SMS Gateway for DeliChow
   - Clear lab/foundation wording if exposed internally

2. How it works
   - DeliChow OS
   - DeliSMS Gateway
   - Android phone
   - Carrier SMS
   - Webhook callback

3. Operational visibility
   - Phone online/offline
   - FCM dispatch
   - SMS send status
   - Webhook event receipt

4. Merchant responsibility notice
   - Phone must stay powered
   - SIM must have load/plan
   - Android battery optimization can affect delivery
   - Carrier/network issues are outside gateway control

5. Integration boundary
   - External service
   - API/webhook integration only
   - Gateway remains separate from DeliChow SaaS core

## Console navigation

Recommended logged-in console nav:

1. Overview
2. Phones
3. Send Test
4. Messages
5. Events
6. Webhooks
7. API Keys
8. Setup Guide
9. Settings

## Overview dashboard

Must show:

- Connected phone count
- Last heartbeat time
- Latest SMS status
- Latest webhook status
- Current API domain
- Current webhook endpoint
- Recent failures
- Setup checklist progress

## Phones page

Must show:

- Phone number
- Device name if available
- Online/offline status
- Last heartbeat
- FCM token updated time
- Battery optimization warning if available
- Linked API key
- Last sent SMS

Failure states:

- Phone offline
- No heartbeat
- FCM SenderId mismatch
- FCM token missing
- API key not attached to phone
- Android app needs reinstall/login

## Send Test page

Must support:

- Send SMS test
- Show selected sending phone
- Show recipient number
- Show message content
- Show immediate API result
- Link to message/event detail

## Messages page

Must show:

- Message ID
- From phone
- Recipient
- Content preview
- Status
- Created time
- Sent time
- Failed reason
- Webhook status

## Events page

Must show:

- Event type
- Message ID
- Dispatch status
- Webhook response if available
- Retry/failure notes

Important events:

- message.phone.delivered
- message.send.failed
- message.expired
- phone.updated

## Webhooks page

Must show:

- Webhook URL
- Secret/signing status
- Last delivery attempt
- Last successful event
- Last failed event
- JWT issuer

Expected issuer for DeliSMS custom domain:

api.sms.delichow.ph

## API Keys page

Must show:

- Key name
- Created time
- Linked phone numbers
- Linked phone IDs
- Last used time if available
- Safe rotate/revoke actions

## Setup Guide

Checklist:

1. Create Phone API Key
2. Install DeliSMS Android APK
3. Set server URL to https://api.sms.delichow.ph
4. Login with Phone API Key
5. Confirm heartbeat
6. Send test SMS
7. Confirm webhook receipt

## Visual direction

Style:

- Clean SaaS admin console
- DeliChow-aligned orange accent
- Neutral background
- Strong status cards
- Clear failure banners
- Tables for messages/events
- Minimal marketing fluff

Tone:

- operational
- clear
- support-friendly
- no exaggerated claims

## Required warning language

Use language like:

- Lab gateway
- External SMS transport
- Android phone must remain online
- Delivery depends on phone, SIM, carrier, and Android background behavior

Do not use language like:

- production-ready
- guaranteed delivery
- enterprise-grade
- fully reliable
- paid-client-ready
- merchant-ready

## Licensing and repo boundary

DeliSMS gateway remains external.

Do not copy AGPL gateway source into the closed-source DeliChow SaaS repo.

DeliChow OS should integrate only through:

- HTTP APIs
- webhooks
- adapter layer
- audit ledger
- notification orchestration

## Implementation rule

Do not redesign everything at once.

Safe order:

1. Audit current pages
2. Remove/rewrite public homepage upstream claims
3. Rebrand visible navigation and dashboard labels
4. Improve console status clarity
5. Rebuild and deploy web
6. Smoke test login, SMS send, webhook receipt
