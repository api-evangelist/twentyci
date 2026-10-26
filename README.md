# TwentyCi (twentyci)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

TwentyCi is a United Kingdom residential property data and home-mover intelligence company. It aggregates roughly 31.5 million UK addresses and tens of billions of data points from hundreds of primary sources into the DOMUS property database, and sells that data to estate agents, lenders, insurers, conveyancers, house builders, retailers and media agencies through the TwentyCi, TwentyEA and TwentyConvey brands. In the UK value chain it sits on the data-supply side rather than the listing side - there is no UK MLS, listings are controlled by the Rightmove and Zoopla portals, and TwentyCi is one of the private aggregators that resells transaction, valuation and home-mover signal on top of that closed market. Its API posture is honest but commercial - TwentyAPI is a genuine RESTful v2 API at https://api.twentyci.co.uk/api/v2 with a live, publicly readable documentation portal covering roughly 57 documented operations across properties, AVM valuation, transaction triggers, agent performance, address matching, schools, housing market metrics and retail propensity - but every endpoint returns 401 without a TwentyCi-issued OAuth 2.0 bearer token, there is no self-serve signup, no published pricing, and the specification download TwentyCi advertises at https://api.twentyci.co.uk/docs/v2/spec.json returns HTTP 404. Access is sales-led and partner-only. RESO is entirely absent - the UK has no MLS or RESO regime and TwentyCi identifies property by UPRN, the Ordnance Survey/GeoPlace Unique Property Reference Number, rather than by any RESO Universal Property Identifier.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/twentyci/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/twentyci/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United Kingdom
- PropTech
- Property Data
- Valuation
- AVM
- Rentals
- Address Data
- Conveyancing
- Homemover Data
- Agent Performance
- Data as a Service

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### TwentyAPI OAuth Token API

The single documented authentication endpoint for TwentyAPI. Exchanges a TwentyCi-issued client_id, client_secret, username and password for a bearer access token and refresh token, which is then presented in an Authorization header as "Bearer <token-key>". TwentyCi labels the scheme OAuth2 with an Implicit flow while documenting a resource-owner password-credentials request body; the only documented scope value is "*". No OpenID Connect discovery document is served.

- **Human URL:** [https://api.twentyci.co.uk/documentation#authorisation-for-api-requests](https://api.twentyci.co.uk/documentation#authorisation-for-api-requests)
- **Base URL:** `https://api.twentyci.co.uk`

#### Tags

- Authentication
- OAuth
- Bearer Token

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-oauth-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#authorisation-for-api-requests)

### TwentyAPI Properties API

The core DOMUS property surface of TwentyAPI. Retrieves property information and detail by UPRN, recent sales and comparable properties for sale in the area, average property values and AVM valuation for a single property, postcode and radius search, property attributes, Google Maps imagery URLs, transaction history, transport links, planning permission data, floor plans, price-per-square-foot comparables and a likely-to-sell propensity signal.

- **Human URL:** [https://api.twentyci.co.uk/documentation#properties](https://api.twentyci.co.uk/documentation#properties)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Property Data
- Valuation
- AVM
- UPRN

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#properties)

### TwentyAPI Agent Performance API

Estate-agent and letting-agent benchmarking built on TwentyCi sales and rental data. Ranks brands by SSTC, new instructions, exchange, PIPA (percentage of initial price achieved) and days from new instruction to SSTC, and returns per-brand statistics for listing value, sold percentage, sale-price difference against all other brands, time to sell, let-agreed volumes, time to let and let ratios.

- **Human URL:** [https://api.twentyci.co.uk/documentation#agent-performance](https://api.twentyci.co.uk/documentation#agent-performance)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Agent Performance
- Benchmarking
- Rentals

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#agent-performance)

### TwentyAPI Trigger Information API

Home-mover event triggers - the transaction-lifecycle signals TwentyCi is built on. Retrieves a specific trigger, lists properties by trigger type (including properties with no UPRN), and returns the trigger history for a property.

- **Human URL:** [https://api.twentyci.co.uk/documentation#trigger-information](https://api.twentyci.co.uk/documentation#trigger-information)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Homemover Data
- Triggers
- Events

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#trigger-information)

### TwentyAPI Categories API

Lists the attribute categories available for a property and returns a specific category, providing the vocabulary that the Properties API's attribute endpoints are organised around.

- **Human URL:** [https://api.twentyci.co.uk/documentation#categories](https://api.twentyci.co.uk/documentation#categories)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Categories
- Metadata

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#categories)

### TwentyAPI Address Match API

Partial address matching. Submits a fragmentary or unstructured UK address to a match-address process and resolves it against TwentyCi's addressing layer, the capability marketed as AddressMaster / Inscriptio address enhancement.

- **Human URL:** [https://api.twentyci.co.uk/documentation#address-match](https://api.twentyci.co.uk/documentation#address-match)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Address Data
- Matching
- Data Quality

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#address-match)

### TwentyAPI Schools API

Returns nearby schools for a given UK postcode, one of the neighbourhood-context datasets TwentyCi layers onto a property record.

- **Human URL:** [https://api.twentyci.co.uk/documentation#schools](https://api.twentyci.co.uk/documentation#schools)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Schools
- Location Data

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#schools)

### TwentyAPI UK Housing Market Metrics API

Aggregate UK housing market metrics for a specified timeframe - new instructions, SSTCs (sold subject to contract) and PCDs (predicted to complete date) - the market-level view behind TwentyCi's published Property and Homemover Report.

- **Human URL:** [https://api.twentyci.co.uk/documentation#uk-housing-market-metrics](https://api.twentyci.co.uk/documentation#uk-housing-market-metrics)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Market Data
- Housing Market
- Analytics

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#uk-housing-market-metrics)

### TwentyAPI This is Now API

"This is Now" retail propensity to buy goods. Local and national search endpoints returning consumer propensity signal derived from home-mover events, aimed at retailers and media agencies targeting people who have just moved.

- **Human URL:** [https://api.twentyci.co.uk/documentation#this-is-now-retail-propensity-to-buy-goods](https://api.twentyci.co.uk/documentation#this-is-now-retail-propensity-to-buy-goods)
- **Base URL:** `https://api.twentyci.co.uk/api/v2`

#### Tags

- Retail
- Propensity
- Marketing Data

#### Properties

- [OpenAPI](openapi/twentyci-twentyapi-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.twentyci.co.uk/documentation#this-is-now-retail-propensity-to-buy-goods)

## Common Properties

- [Website](https://www.twentyci.co.uk/)
- [Documentation](https://api.twentyci.co.uk/documentation)
- [Contact](https://www.twentyci.co.uk/contact/)
- [About](https://www.twentyci.co.uk/about-us/)
- [Blog](https://news.twentyci.co.uk/blog)
- [BlogRSS](https://www.twentyci.co.uk/feed/)
- [PrivacyPolicy](https://www.twentyci.co.uk/privacy-policy/)
- [CookiePolicy](https://www.twentyci.co.uk/cookies-policy/)
- [SustainabilityPolicy](https://www.twentyci.co.uk/sustainability-policy/)
- [Careers](https://www.twentyci.co.uk/careers/)
- [LinkedIn](https://www.linkedin.com/company/twentyci/)
- [Twitter](https://twitter.com/TwentyCi)
- [Facebook](https://www.facebook.com/TwentyCi)
- [GitHubOrganization](https://github.com/twentyci)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
