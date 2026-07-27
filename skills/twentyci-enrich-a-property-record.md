---
name: Enrich a property record with neighbourhood context
description: Layer TwentyCi's attribute categories, transport links, planning applications, schools, floor plans and Street View imagery onto a single UPRN-keyed property record.
api: openapi/twentyci-twentyapi-openapi.json
operations:
- obtain_a_list_of_categories_for_a_property
- obtain_a_specific_category_for_a_property
- property_attributes
- get_properties_property_category_category
- obtain_transport_links_near_a_property
- obtain_planning_permission_data_near_a_property
- schools
- obtaining_a_google_maps_url_for_a_property
- get_properties_uprn_floor_plans
- likely_to_sell
generated: '2026-07-26'
method: generated
source: https://api.twentyci.co.uk/documentation#properties
---

# Enrich a property record with neighbourhood context

Prerequisite: a cached bearer token and a resolved UPRN — see `twentyci-authenticate.md` and `twentyci-value-a-property.md`.

## Step 1 — Learn the attribute vocabulary first

`obtain_a_list_of_categories_for_a_property` — `GET /categories`

Categories are the vocabulary the property attribute endpoints are organised around. Fetch the list once and cache it; do not hardcode category names. The response is JSON:API-*shaped* (`{"id":1,"type":"category","attributes":{"name":"Foo"}}`) but is not JSON:API — the media type is `application/json`, the envelope adds a `message` member, and there are no `links`, `relationships` or `included`.

`obtain_a_specific_category_for_a_property` — `GET /categories/{category}` — one category.

This is a paginated collection: pass `page` and read `meta.pagination.last_page`.

## Step 2 — Pull the attributes

- `property_attributes` — `GET /properties/{property}/category/_all` — every attribute across every category in one call. Prefer this over looping categories.
- `get_properties_property_category_category` — `GET /properties/{property}/category/{category}` — one category's attributes, when you only need a slice.

## Step 3 — Layer the neighbourhood context

Each of these is a separate call against the same UPRN or its postcode — TwentyAPI has no `expand`, `include` or sparse-fieldset parameter, so context is composed client-side by fanning out:

- `obtain_transport_links_near_a_property` — `GET /properties/{property}/transport-links`
- `obtain_planning_permission_data_near_a_property` — `GET /properties/{uprn}/plannings` — paginated.
- `schools` — `GET /nearby-places/schools` — takes a `postcode`, not a UPRN.
- `obtaining_a_google_maps_url_for_a_property` — `GET /properties/{property}/image` — returns a generated Google Maps / Street View URL, not image bytes. Respect Google's terms when you render it.
- `get_properties_uprn_floor_plans` — `GET /properties/{uprn}/floor-plans` — listed on the section index with no parameter documentation at all; the contract is undocumented and must be discovered empirically.

Fan these out concurrently, but conservatively: there is no published rate limit and HTTP 503 ("overloaded with requests") is the only backpressure signal you will get.

## Step 4 — Add the propensity signal

`likely_to_sell` — `GET /{uprn}/likely-to-sell`

TwentyCi publishes this route with no resource segment — literally `api/v2/{uprn}/likely-to-sell`. Transcribed verbatim, not corrected. It returns a propensity signal, which is a model output: present it as a likelihood with its provenance, never as a fact about the owner's intentions.

## Data-handling obligations

This flow assembles a rich profile of an identifiable UK address. TwentyCi is a UK data controller registered with the Information Commissioner's Office under numbers Z9319492 and Z2201604 and states it complies with the relevant data protection regulations; your use of the data is governed by your commercial agreement with TwentyCi and by UK GDPR in your own right. Enriching, storing and re-purposing home-mover and property data has a lawful-basis question attached to it — do not treat the API's willingness to answer as permission to retain or re-sell. TwentyCi's Group Data Protection Officer is reachable at dataprotection@twentyci.co.uk.

## Conventions that bite here

- **Pagination** is `page` + `meta.pagination` on categories and plannings; other sub-resources document none.
- **404 is ambiguous** — unknown route or absent record.
- **Two error envelopes** coexist; parse both. See `errors/twentyci-problem-types.yml`.
