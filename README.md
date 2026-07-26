# TwentyCi (twentyci)

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
