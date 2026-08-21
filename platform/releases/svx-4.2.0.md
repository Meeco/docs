# SVX 4.2.0 Release Notes

**Software Release Date:** 20 August 2026

**Summary:**

This is a feature release for the SVX Platform: a new setting lets you control the precision of credential issuance timestamps, which also closes a privacy gap where credentials issued together in a batch could be linked back to the same issuance via their time claims.

## New Features

### Issuance: control credential timestamp precision

A new runtime setting, `issuer.issuance_policy.time_claims_granularity` (integer seconds, minimum `1`, default `3600`), controls how precisely an issued credential's time claims are stamped. It rounds `iat`, `nbf` and `exp` (and, for `mso_mdoc`, `signed`/`validFrom`/`validUntil`) down to the nearest multiple of this many seconds from the Unix epoch. This applies to every credential issued via `POST /issuer/credential`, not only when several come out of one request.

The default of `3600` means a credential's timestamps land on the hour rather than the exact second it was issued.

```json
PUT /system/settings/issuer
{
  "issuance_policy": {
    "time_claims_granularity": 1
  }
}
```

Also configurable from the issuance policy section of the Admin UI.

This setting also closes a privacy gap in batch issuance: previously, every credential produced by a single `POST /issuer/credential` call got its own precise issuance instant, so a request that produced several credentials at once left a cluster of near-identical, high-entropy `iat`/`nbf`/`exp` values (and `signed`/`validFrom`/`validUntil` for `mso_mdoc`) that let a colluding verifier work out which presentations came from the same issuance and correlate them back to one holder. All credentials produced by one request now share a single issuance instant, rounded down using `time_claims_granularity`, with expiry and validity claims derived from that same rounded instant; the `jwt_vc_json` `issuanceDate` claim is no longer stamped to millisecond precision either. This satisfies [RFC 9901 §10.1](https://www.rfc-editor.org/rfc/rfc9901#section-10.1) conformance requirement.

## Improvements

- The configuration JSON schema now documents a few OAuth/OIDC client settings that were already supported but not written down anywhere.

## Bug Fixes

### Transaction data: type-specific extra properties allowed

`POST /verifier/requests` previously rejected any `transaction_data` entry containing properties beyond `type` and `credential_ids`, since each entry was validated against a fixed shape. It now only requires a non-empty `type` string and a non-empty `credential_ids` array, so the type-specific fields a given `transaction_data` type defines (per OpenID4VP) pass through instead of causing a `400`.

```json
{
  "transaction_data": [
    {
      "type": "urn:example:tx-auth",
      "credential_ids": ["cred_1"],
      "amount": "125.00",
      "currency": "EUR"
    }
  ]
}
```

Previously `amount` and `currency` here would have been rejected; now they're preserved and returned unchanged on `GET /verifier/requests/{id}` and in the signed request JWT.

- Metrics requests no longer get written to the logs, cutting down on noise.
