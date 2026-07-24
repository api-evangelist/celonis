---
name: Subscribe to Celonis Knowledge Model events
description: Create an event subscription against a Knowledge Model trigger and consume its events with pause, resume and replay.
api: openapi/celonis-subscription-openapi.yaml
operations: [getTriggers, getTriggerDetails, createSubscription, getSubscriptions, nextPageFromRemainingData, pauseSubscription, resumeSubscription, replaySubscriptionFromAnOffset, unsubscribe]
---

# Subscribe to Celonis Knowledge Model events

Stream process events by subscribing to a Knowledge Model trigger via the Celonis Event Subscription API.

## Auth
- Base URL: `https://{team_domain}.{realm}.celonis.cloud`, `Authorization` header (OAuth 2.0 Bearer preferred). See `authentication/celonis-authentication.yml`.

## Steps
1. **Find triggers** — `getTriggers` (`GET /intelligence/api/knowledge-models/{km_id}/triggers`); inspect one with `getTriggerDetails` (`.../triggers/{trigger_id}`).
2. **Create a subscription** — `createSubscription` (`POST /intelligence/api/knowledge-models/{km_id}/triggers/{trigger_id}/subscriptions`). Keep the returned `subscription_id`.
3. **List your subscriptions** — `getSubscriptions` (`GET /intelligence/api/subscriptions`).
4. **Consume events** — `nextPageFromRemainingData` (`PATCH /intelligence/api/subscriptions/{subscription_id}/events`) to pull the next page of remaining event data.
5. **Control the stream** — `pauseSubscription` / `resumeSubscription` (`.../pause`, `.../resume`); recover missed data with `replaySubscriptionFromAnOffset` (`.../replay`).
6. **Clean up** — `unsubscribe` (`DELETE /intelligence/api/subscriptions/{subscription_id}`).

## Notes
- Delivery is pull-based (poll for remaining data / replay by offset); email notifications are also available.
- Rate-limit and error conventions match the rest of the platform — see `conventions/celonis-conventions.yml` and `errors/celonis-problem-types.yml`.
