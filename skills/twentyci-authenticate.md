---
name: Authenticate against TwentyAPI and make a first call
description: Mint a TwentyCi OAuth 2.0 bearer token from issued credentials, cache it correctly, and prove it works with a single property lookup.
api: openapi/twentyci-twentyapi-oauth-openapi.json
apis:
- openapi/twentyci-twentyapi-oauth-openapi.json
- openapi/twentyci-twentyapi-openapi.json
operations:
- issueTwentyApiToken
- property_information_by_uprn
generated: '2026-07-26'
method: generated
source: https://api.twentyci.co.uk/documentation#authorisation-for-api-requests
---

# Authenticate against TwentyAPI

## Before you start

You cannot get credentials yourself. TwentyCi has no signup route (`/register`, `/signup` and `/sign-up` all return 404), no published pricing, no free tier and no sandbox. Four secrets — `client_id`, `client_secret`, `username`, `password` — are issued by TwentyCi under a commercial data agreement reached through sales. If you do not have all four, stop here and route the user to https://www.twentyci.co.uk/contact/. Do not attempt to call the API anonymously expecting a demo response: every operation returns HTTP 401.

## Step 1 — Mint a token

`issueTwentyApiToken` — `POST https://api.twentyci.co.uk/oauth/token`

Send a JSON body containing all six documented fields:

- `client_id` (required)
- `client_secret` (required)
- `username` (required)
- `password` (required)
- `grant_type` — the only valid value is `password`
- `scope` — the only valid value is `*`

Headers: `Content-Type: application/json` and `Accept: application/json`.

A success returns TwentyCi's documented shape:

```json
{
    "token_type": "Bearer",
    "expires_in": 1296000,
    "access_token": "your_access_token",
    "refresh_token": "your_refresh_token"
}
```

Note the flow contradiction so you are not confused by the docs: TwentyCi's own security-scheme table labels this "OAuth2 / Flow: Implicit" while documenting a resource-owner password-credentials request on the same page. The request body is what actually runs. Treat it as a password grant.

## Step 2 — Cache the token, do not re-mint

`expires_in` is 1,296,000 seconds — 15 days. Cache the access token for its full lifetime and use the `refresh_token` when it lapses. TwentyCi publishes no rate limits and no quota headers, so there is no signal telling you when re-minting becomes abusive; minting once per request is both wasteful and unmeasurable. Store all four issued secrets and the token itself as secrets — the password grant means a leaked credential set is a full account compromise, and the single wildcard scope `*` means there is no least-privilege token to fall back on.

## Step 3 — Prove it with one call

`property_information_by_uprn` — `GET https://api.twentyci.co.uk/api/v2/properties/{uprn}`

Headers on every call:

```
Authorization: Bearer <token-key>
Content-Type: application/json
Accept: application/json
```

A success returns the property's full address, postcode, coordinates, AVM price, AVM minimum and maximum, and the model's confidence interval.

## Handling failure

- **401** — `{"message":"Unauthenticated.","error":{"status":true,"messages":[]}}`. Missing, malformed or expired token. Refresh, then re-mint.
- **403** — entitlement, not authentication. A valid token does not imply access to every TwentyAPI product family; families are licensed per agreement. Escalate to the TwentyCi account contact, do not retry.
- **404** — TwentyAPI returns 404 both for unknown routes and for absent records. Check the UPRN exists before assuming a routing bug.
- **422** — read `error.messages`, which is keyed by the offending request field.
- **503** — TwentyCi's own wording is "overloaded with requests. Try again later." This is the closest thing to a rate-limit signal in the whole API. Back off exponentially; there is no `Retry-After` and no documented policy.

Expect two different error envelopes in the same API — `{message, error:{status, messages}}` and `{message, success, errors}` — and note that the live 401 types `messages` as an **array** while the documented 422 types it as an **object**. Parse defensively. See `errors/twentyci-problem-types.yml`.
