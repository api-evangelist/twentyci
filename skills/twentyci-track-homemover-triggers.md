---
name: Track home-mover triggers
description: Poll TwentyCi's home-mover transaction lifecycle events by trigger type and postcode, reconcile them to properties by UPRN, and handle the no-UPRN tail — with no webhooks anywhere in the API.
api: openapi/twentyci-twentyapi-openapi.json
operations:
- get_properties_by_trigger_type
- get_no_uprn_properties_by_trigger_type
- trigger_history
- get_trigger_trigger
- property_information_by_uprn
- obtain_all_transactions_for_property_via_uprn
- uk_housing_market_metrics
generated: '2026-07-26'
method: generated
source: https://api.twentyci.co.uk/documentation#trigger-information
---

# Track home-mover triggers

Prerequisite: a cached bearer token — see `twentyci-authenticate.md`.

## Read this before you design anything

**There are no webhooks.** "Trigger" is event *language* over a *polled* REST resource. TwentyAPI has no webhook, callback, subscription, streaming or message-broker surface — zero occurrences of "webhook" in TwentyCi's entire documentation corpus, and no AsyncAPI. If you are building a home-mover alerting product on TwentyCi, you are building a poller, and you own the scheduling, the deduplication and the watermark.

Design accordingly: pick a poll interval, persist the last-seen trigger per postcode sector, and dedupe on your side. TwentyCi publishes no rate limits, so choose a conservative interval and back off hard on HTTP 503 ("overloaded with requests").

## Step 1 — Poll by trigger type and geography

`get_properties_by_trigger_type` — `GET /trigger-type/{typeId}/properties`

Scope with `postcode_district` or `postcode_sector` and bound the window with `max_days`. Poll per sector rather than nationally; a national poll re-reads everything you already have on every cycle.

## Step 2 — Do not drop the no-UPRN tail

`get_no_uprn_properties_by_trigger_type` — `GET /trigger-type/{typeId}/no-uprn-properties`

This operation exists because a real fraction of home-mover events arrive before TwentyCi can bind them to a UPRN. If you poll only step 1 you silently lose the earliest, most commercially valuable signal — the whole point of home-mover data is being early. Poll both, keep the no-UPRN records in a separate holding set, and re-attempt binding on later cycles.

## Step 3 — Resolve a specific trigger

`get_trigger_trigger` — `GET /trigger/{trigger}`

Caveat from the source: TwentyCi's "Obtain a Specific Trigger" documentation page declares the route `GET /categories`, contradicting its own section index. The section index route is the one modelled here. Verify empirically against your credentials before relying on it in production.

## Step 4 — Reconcile to the property

For any trigger that carries a UPRN:

- `trigger_history` — `GET /properties/{property}/triggers` — the full trigger history for that property. Use it to place a new event in the lifecycle rather than treating each poll result as isolated.
- `property_information_by_uprn` — `GET /properties/{uprn}` — address, coordinates, AVM.
- `obtain_all_transactions_for_property_via_uprn` — `GET /properties/{property}/transactions` — historical transactions.

A trigger with no history behind it is a different proposition from the fourth trigger on the same property in eighteen months. Always pull the history before scoring the lead.

## Step 5 — Sanity-check against the market

`uk_housing_market_metrics` — `GET /market/initial-metrics` — new instructions, SSTCs and PCDs for a timeframe. If your poll volume moves sharply, check whether the market moved or your poller broke.

## Conventions that bite here

- **Epoch dates** on `max_days` windows and any timestamp in the payload.
- **404 is ambiguous** — unknown route or absent trigger.
- **No request-id header** exists, so a support escalation about a missed trigger cannot be correlated to a specific call. Log your own request metadata; TwentyCi will not have it.
- **No changelog and no deprecation policy.** If the trigger-type vocabulary changes, nothing will tell you. Assert on `typeId` values you recognise and alert on unknown ones rather than silently dropping them.
