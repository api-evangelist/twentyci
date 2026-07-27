---
name: Read the UK housing market and retail propensity
description: Pull TwentyCi's aggregate UK housing market metrics for a timeframe and the "This is Now" retail propensity-to-buy signal, locally and nationally.
api: openapi/twentyci-twentyapi-openapi.json
operations:
- uk_housing_market_metrics
- local_search
- get_this_is_now_search_national
- average_property_values_by_postcode
- searching_properties_by_postcode_and_radius
generated: '2026-07-26'
method: generated
source: https://api.twentyci.co.uk/documentation#uk-housing-market-metrics
---

# Read the UK housing market and retail propensity

Prerequisite: a cached bearer token — see `twentyci-authenticate.md`.

## Step 1 — The market view

`uk_housing_market_metrics` — `GET /market/initial-metrics`

Returns aggregate UK housing-market metrics for a specified timeframe: **new instructions**, **SSTCs** (sold subject to contract) and **PCDs** (predicted to complete date). This is the market-level view behind TwentyCi's published quarterly Property and Homemover Report.

Timeframes are epoch seconds. This operation is national/market-level, not property-level — do not present it as evidence about any individual address.

## Step 2 — Retail propensity to buy

TwentyCi's "This is Now" product turns home-mover events into consumer propensity signal for retailers and media agencies: people who have just moved buy sofas, broadband and white goods.

- `local_search` — `GET /this-is-now/search` — takes `postcode`, `radius` and `product_type`.
- `get_this_is_now_search_national` — `GET /this-is-now/search-national` — the national view.

Caveat from the source: TwentyCi's "National Search" documentation page declares the *Local Search* route (`/api/v2/this-is-now/search`) rather than the national one — a copy-paste defect in their corpus. The national path modelled here comes from the section index. Verify empirically before relying on it.

## Step 3 — Ground the market view in local prices

- `average_property_values_by_postcode` — `POST /properties/area-value-price` — body takes `postcode`, `radius` and `page`.
- `searching_properties_by_postcode_and_radius` — `POST /properties/area-search` — body takes `postcode`, `radius` and `page`.

Both are paginated: read `meta.pagination.last_page` and loop, rather than assuming one page is the whole answer.

## Interpreting this responsibly

- SSTC is not a completed sale; PCD is a *modelled* date, not a contractual one. TwentyCi's own glossary says so. Carry that qualification into anything you generate.
- Propensity is a model output about a cohort in a geography, not a statement about a named individual. This data is sold for marketing targeting; UK GDPR and the DMA code both apply to how it is used downstream. Your commercial agreement with TwentyCi governs the permitted purposes — the API's willingness to answer is not the permission.
- There is no changelog and no versioning policy beyond the `/api/v2` path segment, so a change in how a metric is computed will arrive silently. Snapshot the metrics you report on with the date you pulled them.

## Conventions that bite here

- **Epoch seconds** for every timeframe parameter.
- **`page` + `meta.pagination`** on the POST search operations; the market and propensity operations document no pagination.
- **503** is the only backpressure signal; no rate limit is published.
