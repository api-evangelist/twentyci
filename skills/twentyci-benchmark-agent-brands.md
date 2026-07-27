---
name: Benchmark estate and letting agent brands
description: Rank agent brands on TwentyCi sales and rental performance — SSTC, new instructions, PIPA, time to sell, let-agreed and let ratios — and pull the per-brand statistics behind a ranking.
api: openapi/twentyci-twentyapi-openapi.json
operations:
- list_of_best_agents
- brands_ranked_by_sstc_s
- brands_ranked_by_new_instructions
- brands_ranked_by_pipa
- brands_ranked_by_days_from_new_instruction_to_sstc
- sstc_statistics_for_a_brand
- new_instructions_statistics_for_a_brand
- days_to_sstc_for_a_specific_brand
- days_to_sstc_for_all_brands
- difference_in_sold_price_for_a_property_for_a_specific_brand_and_all_other_brands
- difference_in_pipa_for_a_property_for_a_specific_brand_and_all_other_brands
- properties_sales_ratio_for_a_given_brand
- brands_ranked_by_let_agreed
- let_agreed_statistics_for_a_brand
- properties_let_ratio_for_a_given_brand
generated: '2026-07-26'
method: generated
source: https://api.twentyci.co.uk/documentation#agent-performance
---

# Benchmark estate and letting agent brands

Prerequisite: a cached bearer token — see `twentyci-authenticate.md`.

## Vocabulary you must get right

From TwentyCi's own glossary:

- **SSTC** — Sold Subject to Contract. An offer accepted, sale not yet legally finalised.
- **PIPA** — Percentage of Initial Price Achieved: the change from initial listing price to final sale price.
- **PCD** — Predicted to Complete Date.

Never present SSTC as a completed sale, and never present PIPA as a discount without saying which direction it runs.

## Step 1 — Pick the ranking

Sales rankings:

- `brands_ranked_by_sstc_s` — `GET /agent-performance/rankings/sstc`
- `brands_ranked_by_new_instructions` — `GET /agent-performance/rankings/new-instructions`
- `brands_ranked_by_pipa` — `GET /agent-performance/rankings/percentage-of-initial-price-achieved`
- `brands_ranked_by_days_from_new_instruction_to_sstc` — `GET /agent-performance/rankings/days-to-sstc`

Rental rankings:

- `brands_ranked_by_let_agreed` — `GET /rentals/agent-performance/rankings/let-agreed`

Or start from `list_of_best_agents` — `GET /agent/best` — when the user has no metric preference.

## Step 2 — Scope the ranking properly

This is the step that decides whether the answer is meaningful. The rankings accept a geographic and a market scope: `postcode_areas`, `postcode_districts`, `postcode_sectors`, `regions`, `price_bands`, `property_types`, `propsubtype`, `bedroom`, `min_price` and `max_price`, plus `limit`.

A national ranking of every brand is almost never the question a user is really asking. Scope to the postcode districts and price band that match the property or the client's patch, and say in the answer exactly which scope you applied. Two brands are not comparable across different scopes.

`limit` caps the result set. There is no documented maximum and no `meta.pagination` on the ranking operations — set `limit` explicitly rather than relying on a default TwentyCi does not publish.

## Step 3 — Pull the per-brand detail

Once you have a `brand_id` from the ranking:

- `sstc_statistics_for_a_brand` — `GET /agent-performance/brand/sstc`
- `new_instructions_statistics_for_a_brand` — `GET /agent-performance/brand/new-instructions`
- `days_to_sstc_for_a_specific_brand` — `GET /agent-performance/brand/time-to-sell`
- `properties_sales_ratio_for_a_given_brand` — `GET /agent-performance/brand/sold-percentage`
- `difference_in_sold_price_for_a_property_for_a_specific_brand_and_all_other_brands` — `GET /agent-performance/brand/property-sale-difference`
- `difference_in_pipa_for_a_property_for_a_specific_brand_and_all_other_brands` — `GET /agent-performance/brand/percentage-of-initial-price-achieved`

Rental equivalents: `let_agreed_statistics_for_a_brand` (`GET /rentals/agent-performance/brand/let-agreed`) and `properties_let_ratio_for_a_given_brand` (`GET /rentals/agent-performance/brand/let-percentage`).

## Step 4 — Compare against the market, not just against rank

`days_to_sstc_for_all_brands` — `GET /agent-performance/brand/time-to-sell-all-brands` — gives the all-brand baseline. A brand ranked first on volume can still be slower and achieve less of the asking price than the market. Report the brand figure and the all-brand figure together; the two "difference" operations above exist precisely because the comparison is the product.

## Conventions that bite here

- **`limit`, not `page`.** The Agent Performance family uses `limit` while the Properties family uses `page` / `per_page`. There is no `meta.pagination` block here.
- **Epoch dates** on any timeframe parameter.
- **403 means entitlement.** Agent Performance is licensed separately from the property data; a token that reads properties may not read rankings. Do not retry a 403.
- **Six rental operations are documented only as section-index entries** with no parameter documentation at all — `get_rentals_agent_performance_brand_time_to_let`, `get_rentals_agent_performance_brand_time_to_let_all_brands` and `get_rentals_agent_performance_brand_let_percentage_all_brands` among them. They are recorded in the spec with zero asserted parameters because TwentyCi asserts none. Probe them with the same scoping parameters the documented siblings use, and expect to discover the contract empirically.
