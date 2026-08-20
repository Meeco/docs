# SVX 4.1.0 Release Notes

**Software Release Date:** 7 August 2026

**Summary:**

This is a feature release for the SVX Platform, led by several new Wallet capabilities, alongside smaller improvements and fixes.

## New Features

- SVX Verify sessions can now report the specific cryptographic key a presentation was bound to, making it easier for integrators to confirm proof of possession.
- Certificate management now shows which signing key a managed certificate belongs to, and certificate listings support filtering and pagination.
- Signing requests can now accept base64url-encoded input, in addition to plain text.
- Session data used by the SVX IDP can now be stored in Postgres as an alternative to Redis. Redis remains the recommended option for production deployments; the Postgres option is intended to simplify deployment for research and development purposes.
- SVX IDP: OAuth clients can now be registered as native or web applications via `POST /clients` and `PUT /clients/{id}` (per the [OIDC Dynamic Client Registration](https://openid.net/specs/openid-connect-registration-1_0.html#ClientMetadata) `application_type` metadata), defaulting to web. This lets the SVX IDP apply the correct security requirements for native app integrations.
- **Transaction Data support** (per the [OpenID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) specification): Verifiers can now attach additional context to a presentation request, and wallets can restrict which types of this data they are willing to accept. Verifiers can confirm that the credential presentation returned by a wallet was genuinely bound to the requested context, adding an extra layer of assurance to verification flows.


### How to list certificates

`GET /system/certificates` now returns the `kid` of the signing key a `managed` certificate is bound to, alongside the existing `key_name`, and accepts query parameters for filtering, sorting and pagination.

**Filtering by type:** pass `type` to narrow the list to `managed` (bound to a managed signing key), `trust_anchor` (imported for external chain validation), or `iaca` (Issuer Authority Certificate Authority). An unrecognised value is rejected with `400`.

```bash
curl -sS -G "$SVX_WALLET_BASE_URL/system/certificates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  --data-urlencode "type=managed" | jq
```

Each entry now carries the signing key it belongs to — `key_name` and `kid` (the JWK thumbprint of the key's public JWK) are populated for `managed` certificates, and are `null` for `trust_anchor` and `iaca` ones:

```json
{
  "certificates": [
    {
      "id": "6f0a2c4e-3c1a-4d2b-9a4f-8e1b7c5d0a11",
      "type": "managed",
      "key_name": "CredentialKey",
      "kid": "NzbLsXh8uDCcd-6MNwXF4W_7noWXFZAfHkxZsRGC9Xs",
      "x5c": ["MIIB4TCCAYigAwIBAgIU..."],
      "fingerprint256": null,
      "created_at": "2026-08-07T02:14:33.512Z"
    }
  ],
  "meta": {
    "order_by": "created_at",
    "order": "DESC",
    "order_from_params": true,
    "per_page": 50,
    "per_page_from_params": true,
    "records_count": 1,
    "page": 1,
    "page_count": 1
  }
}
```

**Paginating and sorting:** `page` and `per_page` select the page, and `order_by` with `order` control the sort. `order_by` accepts `created_at` (the default) and `type`; `order` accepts `ASC` or `DESC`, defaulting to `DESC` — so without any parameters you get the 50 most recently imported certificates. `per_page` is clamped to the range 1–50, and a non-numeric `page` or `per_page`, or a `page` below 1, is rejected with a `400` `invalid_pagination_param` error.

```bash
# Second page of 10, oldest first
curl -sS -G "$SVX_WALLET_BASE_URL/system/certificates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  --data-urlencode "page=2" \
  --data-urlencode "per_page=10" \
  --data-urlencode "order_by=created_at" \
  --data-urlencode "order=ASC" | jq
```

Sorting by `type` groups the three kinds together, which is a convenient way to audit which signing key each managed certificate is bound to:

```bash
curl -sS -G "$SVX_WALLET_BASE_URL/system/certificates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  --data-urlencode "order_by=type" \
  --data-urlencode "order=ASC" | jq '.certificates[] | {type, key_name, kid, created_at}'
```

Use `meta.records_count` and `meta.page_count` to drive iteration over larger sets. The Certificates API guide covers the full certificate lifecycle, from creating a CSR through to importing the signed chain.

### How to use Transaction Data

**On the verifier side:** you can now add an optional `transaction_data` array to `POST /verifier/requests` (each entry: `{ type, credential_ids }`), in order to bind a presentation request to extra context — for example, "this presentation also authorizes payment X" — scoped to specific credentials via `credential_ids`. That context is echoed back on `GET /verifier/requests/{id}` and embedded in the signed request JWT served from `GET /verifier/requests/{id}/jwt`. Once a response comes back, `POST /verifier/requests/{id}/responses` automatically checks it against the hash the wallet produced, giving you assurance the presentation was genuinely made in response to that specific context — not just any valid credential.

**On the wallet side:** a new runtime setting, `wallets.trust.allowed_transaction_data_types`, lets a wallet declare which `transaction_data` types it's willing to honor, so it can decline anything unrecognized rather than blindly accepting arbitrary context from a verifier. This check happens at `POST /:walletId/send`, and the same `transaction_data` is also surfaced on `GET /:walletId/send/{state}`, `GET /:walletId/send`, and `POST /:walletId/send/set_credentials`, so a client app can inspect what it's being asked to bind to at any point in the flow. Then, at `POST /:walletId/send/submit`, the hash-binding itself happens automatically — no extra input needed from the caller — by including `transaction_data_hashes` in the Key Binding JWT of any `dc+sd-jwt` credential the context was scoped to.

Existing integrations that don't use `transaction_data` are unaffected — it's entirely optional.

## Improvements

- Failed SVX Verify presentations now include a more detailed reason, making it easier to understand why a verification did not succeed.
- The demo issuance and verification pages have moved into the main dashboard; the previous standalone demo pages have been retired.
- SVX Verify no longer displays a country flag or country name for identity providers and wallets.
- Credential template colors must now be valid hex colors; unsupported formats such as gradients are no longer accepted.
- Updating a credential template's validity settings now fully replaces the previous settings, so clearing a value (such as removing an expiry) is applied correctly.
- The default signing algorithm used for admin and access tokens has changed from ES256 to RS256.
- Runtime configuration is no longer cached by the browser, so configuration changes take effect immediately.

## Bug Fixes

- Fixed an issue where some internal errors were silently discarded instead of being logged, making certain failures harder to diagnose.
- Fixed the SVX Verify PDF receipt being cut off instead of spanning multiple pages when a presentation contained a lot of information.
- Fixed claim labels in the PDF receipt so that untranslated claims show a readable name instead of a raw technical key.
- Fixed images, such as a portrait photo, not displaying correctly on the SVX Verify success page and in the PDF receipt.
- Fixed an issue where cancelling a verification session at the wrong moment could send the user to a generic error page instead of the expected cancellation page.
- Fixed the "Save & Preview" redirect URL in the SVX Verify configuration screens.
- Fixed an image format (JPEG2000) not rendering correctly in presentation responses.
- Fixed credential issuance failing for templates with validity dates configured.
- Improved reliability of database setup during deployment on managed Postgres providers (such as AWS Aurora) that restrict administrator permissions.
