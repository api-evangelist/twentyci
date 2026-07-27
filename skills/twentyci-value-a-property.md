---
name: Value a UK property with the TwentyCi AVM
description: Resolve a messy UK address to a UPRN, pull the property record and detail, run the Automated Valuation Model, and evidence the number with comparables and recent local sales.
api: openapi/twentyci-twentyapi-openapi.json
operations:
- address_match
- property_information_by_uprn
- property_details_by_uprn
- valuation_of_property_by_uprn_avm
- comparables
- recent_property_sales_in_the_area
- similar_properties_for_sale_in_area
- average_property_values_by_postcode
generated: '2026-07-26'
method: generated
source: https://api.twentyci.co.uk/documentation#properties
---

# Value a UK property with the TwentyCi AVM

Prerequisite: a cached bearer token — see `twentyci-authenticate.md`. Send `Authorization: Bearer <token-key>`, `Content-Type: application/json` and `Accept: application/json` on every call below.

## Step 1 — Get to a UPRN

Everything in TwentyAPI keys on the UPRN, the Ordnance Survey / GeoPlace Unique Property Reference Number. There is no MLS identifier and no RESO Universal Property Identifier — the UK has neither.

If you already have a UPRN, skip to step 2. If you have a free-text or partial address, resolve it first:

`address_match` — `POST /match-address-processes`

This is TwentyCi's Inscriptio / AddressMaster address-enhancement capability exposed as an API. Submit the fragmentary address; use the resolved property as the key for everything downstream. Do not attempt to construct a UPRN yourself.

## Step 2 — Pull the record and the detail

- `property_information_by_uprn` — `GET /properties/{uprn}` — full address, postcode, coordinates, AVM price, AVM minimum, AVM maximum, model confidence interval.
- `property_details_by_uprn` — `GET /properties/{property}/details` — the fuller property detail record.

`property_information_by_uprn` already returns an AVM price. Step 3 exists for when you need the valuation call in its own right or need it for a property you are not otherwise reading.

## Step 3 — Run the AVM

`valuation_of_property_by_uprn_avm` — `POST /propertiesavm2/{property}`

The route really is spelled `propertiesavm2` with no slash before `avm2`. That is TwentyCi's own published route, transcribed verbatim rather than corrected; do not "fix" it to `/properties/avm2/`.

Always carry the **confidence interval** and the **min/max band** through to whatever you present. An AVM point estimate presented without its band is a misrepresentation of the model, and TwentyCi's own glossary is explicit that an AVM is statistical modelling over existing databases, not an inspection.

## Step 4 — Evidence the number

A valuation an agent or lender will act on needs comparables:

- `comparables` — `GET /price-per-square/comparables` — price-per-square-foot comparables.
- `recent_property_sales_in_the_area` — `GET /properties/{property}/recent-sale-in-the-area` — what actually sold nearby.
- `similar_properties_for_sale_in_area` — `GET /properties/{property}/for-sale-in-the-area` — what is currently competing.
- `average_property_values_by_postcode` — `POST /properties/area-value-price` — the postcode-level baseline. Body takes `postcode`, `radius` and `page`.

## Conventions that bite here

- **Pagination.** The area/search operations paginate with `page` in the request body or query and return a `meta.pagination` block with `total`, `last_page`, `per_page` and `current_page`. Loop on `last_page`, not on an empty page. `recent_property_sales_in_the_area` also accepts `per_page`.
- **Filters.** The property search surface accepts `bedrooms`, `bathrooms`, `property_type`, `energy_rating`, `floor_area_band`, `garage`, `driveway`, `ensuite` and `postcode`. Filter server-side rather than over-fetching — you are paying for data volume.
- **Dates are epoch seconds**, per TwentyCi's own glossary.
- **No idempotency contract exists.** These POSTs are read-shaped queries, not writes, so a retry costs you a duplicate query rather than a duplicate record — but there is no `Idempotency-Key` to lean on and no request-id header to correlate a retry with. See `conventions/twentyci-conventions.yml`.
- **404 is ambiguous** — unknown route or absent record. Confirm the UPRN resolved in step 1 before treating it as a bug.
