# SVX 4.3.0 Release Notes

**Software Release Date:** TBD

**Summary:**

This is a feature release for the SVX Platform, headlined by:

- **Credential catalog**: Install ready-made schemas, credential templates and verification templates for standard credentials (mDL, EU PID, German PID, Age Verification, Photo ID, Digital Payment Credential, Mobile Vehicle Registration) in a single step.
- **IETF Token Status List**: Credentials in every format can now be revoked and suspended using the IETF Token Status List, and verifiers cache status lists instead of downloading them for every credential.
- **ISO 18013-5 support for mdoc readers**: SVX can act as a reader certificate authority that issues reader authentication certificates, and can import verifications that mdoc readers performed outside SVX.
- **Key and certificate management**: New Keys and Certificates pages in the Admin UI, and certificate activation and deactivation for renewing signing certificates without downtime.
- **Stricter holder binding verification**: OpenID4VP verification now enforces `require_cryptographic_holder_binding` as the DCQL specification requires, and reports DCQL failures with a dedicated error per kind of mismatch.

It also includes security fixes, several smaller features and many improvements to the Admin UI.

**Important:** This release contains breaking changes. Please review [Breaking Changes](#breaking-changes) before upgrading.

## Breaking Changes

- **Admin dashboard authentication is now static configuration.** `system.admin.auth_method` and `system.admin.idp` have moved from runtime settings to `config.json` and can no longer be edited from the dashboard Settings page. `auth_method` defaults to `passkey`. A migration removes these keys from the stored runtime settings, so **deployments that use OIDC for admin login must set them in `config.json` before upgrading**. Helm users should also remove `admin` from `runtimeBootstrap.systemDocument`.
- **ConnectID must be explicitly enabled.** ConnectID now only runs when `bridge.integrations.connect_id.enabled` is `true`, as SelectID already did. The field is required, so existing `connect_id` blocks in `config.json` need `"enabled": true`. Setting it to `false` while runtime ConnectID settings are saved prevents the server from starting.

  ```json
  {
    "bridge": {
      "integrations": {
        "connect_id": {
          "enabled": true
        }
      }
    }
  }
  ```

- **The production image no longer contains `config/config.json` or `config/runtime.seed.json`.** Deployments must mount `config/config.json`. The Helm chart already does this.
- **Holder binding is now enforced.** OpenID4VP verification previously accepted every presentation regardless of the DCQL query's `require_cryptographic_holder_binding`, and a query that omitted the flag was treated as not requiring binding. The specification default of `true` is now applied, so presentations without holder binding are rejected unless the credential query sets `"require_cryptographic_holder_binding": false`. See [Holder binding enforcement and DCQL error reporting](#verification-holder-binding-enforcement-and-dcql-error-reporting).
- **`mso_mdoc` credential templates must have an expiry.** Creating or updating an mdoc template without `expires_at` or `expires_in` fails with `validation_error`. Issuing from an existing mdoc template without one fails with `mso_mdoc_expiry_required`, instead of falling back to a far-future `9999-01-01` date. The expiry also cannot be later than the signing (DS) certificate's `notAfter`; issuance fails with `mso_mdoc_expiry_exceeds_certificate` otherwise.
- **OpenAPI document and Swagger UI moved.** The OpenAPI document is served at `GET /openapi` and the Swagger UI at `/openapi/ui` (previously `/api-json` and `/api`). The document is always served; `system.swagger_ui_enabled` now controls only the UI.
- **Presentation request fields are nullable.** To support imported ISO 18013-5 requests, `response_type`, `response_mode`, `nonce` and `expires_at` can now be `null` in `GET /verifier/requests` and `GET /verifier/requests/{id}`. This is a type change in the generated SDKs.
- **Digital Credentials API origin.** Authorization response submission no longer derives the Digital Credentials API origin from request headers. It comes from `options.expected_origin` on `POST /verifier/requests/{id}/responses`, falling back to `system.app_host`.
- **SVX Verify sessions require a complete setup.** `POST /verify/sessions` returns `409 svx_verify_not_configured`, with the missing fields in `extra_info.missing_fields`, until the SVX Verify brand name, operating company, terms URL, privacy URL and at least one identity provider are set.
- **Name uniqueness and admin display names.**
  - Credential schema and credential template names are now unique per format, and verification template names unique per type (`pex`, `dcql`, `device_request`), rather than globally.
  - A custom credential configuration id (`config.issuer_credential_configurations_supported_name`) can only be used by one credential template; duplicates are rejected with `credential_template_configuration_id_already_exist`. A migration renames existing duplicates, keeping the id of the oldest template and adding the format suffix (for example `_mso_mdoc`) to the others.
  - `POST /admin/invitations` now requires `display_name`, and admin display names must be unique. A migration names unnamed admins `Admin` and suffixes duplicates (` (2)`, ` (3)`).
  - API key names are limited to 100 characters (previously 255).
- **Issuer-controlled claims cannot be supplied.** Claims the issuer sets itself (for example `nbf`, `exp`, `iat`, `iss`, `status` or `credentialStatus`) are rejected with a `400` naming the claim when supplied in offers or ad-hoc issuance. `jwt_vc_json` and `dc+sd-jwt` schemas can no longer define them, and claims with these names from resource hooks and claim mappings are ignored.
- **Status list changes.** `issuer.issuance_policy.revocable` is deprecated and ignored in favour of the per-template `status_list` setting, and `statusListIndex` in W3C credentials is now a string. See [Status lists](#status-lists-ietf-token-status-list-support).
- **Managed certificates cannot be deleted once they have been active.** `DELETE /system/certificates/{certificate_id}` returns `409 certificate_has_been_active`; deactivate them instead. A migration marks the newest existing certificate per key as active, so all existing managed certificates count as having been active.

## New Features

### Credential catalog

A catalog of standard credentials makes it possible to set up issuance and verification for well-known credential types without writing schemas and DCQL queries by hand. Each catalog entry defines, per format (`dc+sd-jwt`, `mso_mdoc`), the credential schema, credential template defaults and verification presets.

| Entry | Specification | Formats |
| --- | --- | --- |
| mDL | ISO 18013-5 | `dc+sd-jwt`, `mso_mdoc` |
| EU PID | ARF PID Rulebook (Annex 3.01) | `dc+sd-jwt`, `mso_mdoc` |
| German PID | German PID Rulebook 1.0.0 draft (`urn:eudi:pid:de:1`, namespace `eu.europa.ec.eudi.pid.de.1`) | `dc+sd-jwt`, `mso_mdoc` |
| Age Verification | | `dc+sd-jwt`, `mso_mdoc` |
| Photo ID | ISO/IEC TS 23220-4 (data elements of ISO/IEC 23220-2) | `dc+sd-jwt`, `mso_mdoc` |
| Digital Payment Credential | EMV DPC Schema Framework v1.0 draft (`com.emvco.dpc.card`) | `dc+sd-jwt`, `mso_mdoc` |
| Mobile Vehicle Registration | ISO/IEC DTS 7367-2 (doctype `org.iso.7367.1.mVC`) | `mso_mdoc` |

The EU PID rulebook defines no age attestations, so "age over 18" comes from the Age Verification entry. The catalog schemas are a first version and will be refined in future releases.

**Installing from the catalog:** `POST /catalog/{id}/install` creates, in one call, the schema and credential template (`issuance`, default `true`) and/or one verification template per chosen preset (`verification_presets`). Choosing neither returns `catalog_nothing_to_install`. Issuing `mso_mdoc` credentials requires a certificate on `CredentialKey` (`catalog_prerequisite_not_met` otherwise); installing verification templates only does not.

Verification presets are a fixed set, in this order: Age over 18, Age over 18 + portrait, one extra preset per format, Mandatory data elements, and All data elements. The last two are generated from the schema.

**Browsing the catalog:** `GET /catalog` lists the entries together with the credential templates and verification templates (`installed_verification_templates`) installed from them. `GET /catalog/{id}` returns the schema, template defaults, verification presets (with their DCQL queries) and prerequisites for each format. Credential and verification templates installed from the catalog carry a `catalog` object with the entry id, its version and, for verification templates, the preset id.

**Sample data:** each catalog entry has one or more samples, such as a holder, a payment card or a vehicle. `GET /issuer/templates/{id}/samples` (and `GET /demo/issuer/templates/{id}/samples`) returns them with claims in the template's format, valid against its schema, and with dates relative to today. Templates not installed from the catalog, or installed from a different version of their entry, have no samples.

**In the Admin UI:** the new Credentials › Catalog page, also reachable with **Add from catalog** on the Credential Templates page, installs catalog entries. The install sheet shows the schema and templates already installed for the chosen format, pre-selects only what is missing, and prevents installing a part again under the name of an existing template. On the Test Issue page, claims can be filled in from one of the samples of the template's catalog entry.

The catalog can be hidden with the runtime setting `system.credential_catalog.visible` (default `true`, Administration › Settings › System › Credential catalog). When hidden, the Admin UI removes the Catalog page and **Add from catalog**, and the `/catalog` endpoints return `404 catalog_not_visible`. Templates already installed from the catalog keep working.

### Status lists: IETF Token Status List support

SVX now supports the IETF Token Status List ([draft-ietf-oauth-status-list](https://datatracker.ietf.org/doc/draft-ietf-oauth-status-list/)) alongside the W3C Bitstring Status List, so `dc+sd-jwt` and `mso_mdoc` credentials can now be revoked and suspended.

**Choosing a status list:** each credential template selects its status list with `status_list`:

- `ietf_list`: available for all formats; the default for `dc+sd-jwt` and `mso_mdoc`.
- `w3c_list`: `jwt_vc_json` only; the default for `jwt_vc_json`.
- `none`: credentials are not revocable.

The setting is available in the Admin UI template forms. It replaces `issuer.issuance_policy.revocable`, which is now deprecated and ignored.

**Issuing:** credentials issued from `ietf_list` templates reference their status list entry with a `status` claim: top-level in `jwt_vc_json`, always disclosed in `dc+sd-jwt`, and inside the signed MSO in `mso_mdoc`. Entries can be 1, 2, 4 or 8 bits.

**Publishing:** IETF lists are published as Status List Tokens at `GET /issuer/status_list/{id}`, in one of two forms chosen by content negotiation: a JWT (`application/statuslist+jwt`, the default) or a CWT (`application/statuslist+cwt`, COSE_Sign1) when the `Accept` header prefers it. Both are signed with the new `StatusListKey`.

```bash
curl -sS "$SVX_WALLET_BASE_URL/issuer/status_list/$STATUS_LIST_ID" \
  -H "Accept: application/statuslist+cwt" --output status_list.cwt
```

The CWT form carries the certificate chain registered for `StatusListKey` as `x5chain`. For mdoc verifiers to accept it, the `StatusListKey` certificate must be issued by the same IACA as the `CredentialKey` certificate.

**Verifying:** the verifier checks IETF status list entries for all three formats, using the JWT form for `jwt_vc_json` and `dc+sd-jwt` and the CWT form for `mso_mdoc`. It verifies the token's signature, `typ`, that `sub` matches the credential's `uri`, and `exp`. Revoked and suspended credentials fail verification.

- JWT lists are verified with the key the issuer publishes at `/.well-known/jwt-vc-issuer`, selected by `kid`.
- CWT lists follow ISO 18013-5: the CWT must contain an `x5chain`, the key is taken from its leaf certificate, and the chain is accepted only if its top-level certificate is signed by the `certificate` in the MSO's status list reference or, without one, by the issuer of the MSO's certificate (the IACA), found in the MSO or among the registered IACA certificates.

**W3C Bitstring Status List changes:**

- Status list credentials are signed with the new `StatusListKey` and served as `application/vc+jwt`.
- Status list entries are allocated at random positions instead of sequentially, so a credential's index no longer reveals the order in which it was issued. To make this possible, status lists now hold 262,144 (2^18) entries, above the minimum size the W3C specification mandates.
- Status lists are encoded as multibase base64url, and `statusListIndex` in a credential is now a string.

### Status lists: caching and `ttl`

The verifier now caches verified status lists, both W3C and IETF, so it no longer downloads and verifies the same list, and fetches the issuer's keys, for every credential. A list is cached until `iat + ttl` when it has a `ttl`, otherwise for a fallback period, and never past its `exp`.

Published lists now carry a `ttl`: the `ttl` claim (in seconds) of IETF Status List Tokens, and `credentialSubject.ttl` (in milliseconds) of W3C lists.

New runtime settings, also editable in the Admin UI Settings:

- `issuer.status_list.ttl_seconds`: the published `ttl` (default `300`; `0` publishes none).
- `verifier.status_list_cache.enabled`: default `true`.
- `verifier.status_list_cache.fallback_ttl_seconds`: default `300`.

```json
PUT /system/settings/verifier
{
  "status_list_cache": {
    "enabled": true,
    "fallback_ttl_seconds": 600
  }
}
```

### ISO 18013-5: reader authentication certificate authority

SVX can now act as the certificate authority for ISO 18013-5 mdoc readers, issuing the reader authentication certificates that holder wallets use to identify the reader requesting their data.

**Setup:** enable the runtime setting `verifier.reader_ca.enabled` (requires the static `verifier.enabled`). This provisions a new optional ES256 managed key, `ReaderCAKey` (`system.kms.managed_keys.reader_ca`). Its issuing CA certificate is signed outside SVX and imported with `POST /system/certificates/import`. Certificates imported for `ReaderCAKey` must be issuing CA certificates (critical `CA:true`, critical `keyCertSign` and `cRLSign`, a subject key identifier, an ECDSA signature, and CA certificates along the chain); others are rejected with `invalid_reader_ca_certificate`.

**Endpoints**, available while `verifier.reader_ca.enabled` is on (`404` otherwise):

- `POST /verifier/reader/certify` issues reader authentication certificates (ISO 18013-5 Annex B, Table B.6) for a batch of CSRs, signed by `ReaderCAKey`. The CN comes from the CSR (CSRs without one are rejected); C, O and OU come from the issuing CA. Each result, in request order, is either `{ "x5c": [reader certificate, issuing CA, ...] }` or a per-CSR `{ "error": ... }`.
- `GET /verifier/reader/issuing-ca.pem` and `GET /verifier/reader/issuing-ca.der` publish the issuing CA certificate that holder wallets trust, with an `X-Certificate-Fingerprint-SHA256` header.
- `GET /verifier/reader/crl` publishes the certificate revocation list referenced by every reader certificate. It is signed by `ReaderCAKey` and is currently always empty.

The runtime settings `verifier.reader_ca.leaf_validity_hours` (default `24`) and `verifier.reader_ca.max_csrs_per_request` (default `10`) set the reader certificate validity and the batch size. They are editable in the new Reader authentication section of Administration › Settings › Verifier, which also shows the active `ReaderCAKey` certificate or links to importing one. Issued certificates are counted by the new metric `reader_ca_certifications_total{result}`.

### ISO 18013-5: importing verifications performed outside SVX

`POST /verifier/requests/import` imports ISO 18013-5 verifications performed outside SVX, for example by mdoc readers, so they can be stored and reviewed alongside verifications made through SVX. Each record contains `record_reference_id`, `device_request`, `device_response`, `requested_at`, the reported `verification_result` and an optional `session_transcript`, and is stored as an imported presentation request and response with protocol `iso_mdoc`. The verification result is reported by the uploader and is not re-verified by SVX.

- A record without `device_response` and `verification_result` but with a past `expires_at` represents a request that expired unanswered. It is stored as a `pending` imported request without a response, like an expired OpenID4VP request.
- Imports are idempotent on `record_reference_id`, and stored records are never updated. A different record under an existing `record_reference_id` is rejected as `reference_conflict` with the id of the stored record; to replace it, delete it and import again.
- The runtime setting `verifier.request_import.max_records_per_request` (default `100`) caps the number of records per request. It is editable in the new Request import section of Administration › Settings › Verifier.

Presentation requests now have a `protocol` (`openid4vp` or `iso_mdoc`) and an `imported` flag, and `iso_mdoc` requests carry `record_reference_id` and `doc_requests`. Presentation responses gain `device_response` and `session_transcript`. Verification results gain an optional `device_response` part for ISO 18013-5 DeviceResponses (response-level `checks`, `warnings` and `errors`, and `documents` with a `doc_type` each), which holds the reported result of an imported request. `GET /verifier/requests/{id}/jwt` and `POST /verifier/requests/{id}/responses` return `404 not_found` for `iso_mdoc` requests.

**Device request templates:** a new `device_request` verification template type stores `doc_requests`, the JSON (snake_case) equivalent of ISO 18013-5 DocRequests, in which each data element maps to its IntentToRetain flag. These templates cannot be used to create OpenID4VP presentation requests (`presentation_template_type_not_supported`). The dashboard JSON editor offers a Device Request starter and an mDL sample with structure checks.

### Keys and certificates: management in the Admin UI

**Keys:** `GET /system/keys` lists the deployment's KMS keys, including keys retired by rotation, with their algorithm, purpose, adapter, key ID, public JWK, use count, bound certificates (subject, issuer, validity, fingerprint) and creation and retirement dates. The new read-only Administration › Keys page shows an overview of the keys and a detail panel with bound certificates and previous versions, and can copy a key pair's public key as a JWKS.

**Certificates:** the new Administration › Certificates page lists managed, trust anchor and IACA certificates, with a detail panel showing the full chain, copy-as-PEM actions and a link to the bound key. From this page you can import trust anchors, or managed certificate chains bound to a signing key, by pasting PEM, and activate, deactivate and (where allowed) delete certificates.

On the API side:

- `GET /system/certificates/{certificate_id}` returns a single certificate.
- `GET /system/certificates` filters by `type`, `status` (`active` or `inactive`, for managed certificates) and `key_name`, each accepting a comma-separated list.
- Certificate responses from `GET /system/certificates`, `GET /system/certificates/{certificate_id}` and `POST /system/certificates/import` include `chain`: the `x5c` entries parsed for display (subject, issuer, serial number, validity, public key, subject alternative names, CA and self-signed flags, SHA-256 fingerprint).

### Certificates: activation and renewal

A signing key can now have several managed certificates, for example the current certificate and its renewal, but only one of them is active: the one attached as `x5c` to what the key signs. The first certificate imported for a key is active on import; later ones are imported inactive. `POST /system/certificates/{certificate_id}/activate` and `POST /system/certificates/{certificate_id}/deactivate` switch which one is active, so a renewed certificate can be imported ahead of time and activated when needed.

```bash
curl -sS -X POST "$SVX_WALLET_BASE_URL/system/certificates/$CERTIFICATE_ID/activate" \
  -H "Authorization: Bearer $ACCESS_TOKEN" | jq '{id, active, activated_at}'
```

Certificate responses include `active`, `activated_at` and `deactivated_at`.

### Verification: holder binding enforcement and DCQL error reporting

OpenID4VP verification now enforces the DCQL query's `require_cryptographic_holder_binding`. Previously the requirement was always considered satisfied, and the specification default (`true`) was not applied when a query omitted the flag. Holder binding is now derived from the presentation itself:

- `dc+sd-jwt`: a Key Binding JWT, verified against the credential's `cnf.jwk`.
- `jwt_vc_json`: a holder key or subject DID tied to the presentation signer.
- `mso_mdoc`: device authentication.

Verification results record a new `holder_binding` check. Verifiers that do not need binding can set `"require_cryptographic_holder_binding": false` on the credential query. On the wallet side, `set_credentials` now returns an error when a selected credential cannot satisfy the query's holder binding requirement.

DCQL verification failures are now reported as a distinct error per kind of mismatch, each listing the affected credential queries, instead of a single `failed_to_validate_presentation_response`:

- `dcql_presentation_missing_cryptographic_holder_binding`
- `dcql_presentation_does_not_match_credential_metadata`
- `dcql_presentation_claims_do_not_satisfy_query`
- `dcql_presentation_issuer_is_not_a_trusted_authority`
- `dcql_presentation_missing_for_credential_query`
- `dcql_required_credential_set_not_satisfied`

A credential query that received no presentation at all is now named in the error.

### Ad-hoc issuance: per-issuance validity

Ad-hoc issuance (`POST /issuer/credentials`) accepts optional `valid_at` and `expires_at` (ISO 8601) to set the validity of that issuance, overriding the credential template's `valid_in`/`valid_at` and `expires_in`/`expires_at`. They set `nbf`/`exp` for `dc+sd-jwt` and `jwt_vc_json`, and `ValidityInfo.validFrom`/`validUntil` for `mso_mdoc`. A side that is not set still comes from the template. Requests whose validity would end before it starts, or has already ended, are rejected with `invalid_request`.

The response now includes the issuer credential `id` of each credential (`credentials: [{ id, credential }]`), so it can be used directly with endpoints such as `PATCH /issuer/credentials/{id}/status`.

### Digital Credentials API

- `POST /verifier/requests/{id}/responses` accepts an optional `options.expected_origin`. It is used as the `origin:` audience and as the `mso_mdoc` session transcript origin for the `dc_api` and `dc_api.jwt` response modes, and defaults to `system.app_host`.
- The credential offer registered with `POST /wallets/{walletId}/receive` can include `credential_issuer_metadata` and `authorization_server_metadata` inline. When present, they are used instead of `.well-known` discovery. This is required for offers delivered through the Digital Credentials API, where the wallet-selection matcher has no network access.
- **Experimental:** the demo Issue page offers Digital Credentials API issuance when the browser supports it. Errors reported by the browser are shown. In cross-device flows the browser can report an error even when issuance succeeded, so check the wallet; the same-device flow works as expected.

### Other new features

- **Credential offer URL per offer:** `POST /issuer/offers` and `POST /demo/issuer/offers` accept an optional `credential_offer_endpoint` that overrides `issuer.issuance_policy.credential_offer_endpoint` for that offer. The Test Issue page has a matching Credential offer URL field.
- **Search:** the View Credentials, Verification Requests, API Keys and Administrator Accounts pages have a search box. `GET /issuer/credentials`, `GET /application/applications`, `GET /admin/accounts` and `GET /admin/invitations` accept a `search` query parameter (up to 50 characters) that matches the name or ID case-insensitively, and the `GET /verifier/requests` search now also matches the request ID. `%` and `_` are matched literally.
- **Filtering wallet presentations by status:** `GET /wallets/{walletId}/send` filters presentation states by submission status with the `status` query parameter (`pending`, `failed`, `success`), as a comma-separated list or a repeated parameter. Other values are rejected with `invalid_status_filter_value`.
- **Admin display names:** display names can be edited with `PATCH /admin/accounts/{id}` and in the Admin UI, and admin passkeys show the display name instead of a UUID.
- **API documentation link:** Devtools › API Documentation opens the Swagger UI in a new tab when `system.swagger_ui_enabled` is set.
- **SVX Verify contact details:** the verifier contact phone number (`svx_verify.display.verifier_information.contact_information.phone`) and contact email are now optional; the receipt PDF shows "Phone number not available" or "Email not provided" in their place. A new optional HTTPS `contact_url` is shown as a link on the receipt.
- **CSP form targets:** the public SVX Verify (`/verify/**`) and authorization (`/interaction/**`) pages now send a Content Security Policy `form-action` directive, so their forms can submit only to `'self'` and allowed targets. Known redirect targets (the SVX Verify Bridge URL, the session return URL, the client `redirect_uri` and Bridge IdP issuers) are added automatically. Extra targets can be added with the runtime setting `system.security.headers.content_security_policy.form_action` (default `["'self'"]`, Settings › System › Security headers).
- **Test Verify page:** can omit `state` from the request, set a redirect URI (`verifier_redirect_uri`), and set an optional request name.

## Improvements

### Admin UI

- **Navigation:** the sidebar uses the SVX Design System icons and is ordered Dashboard, Credentials, SVX Verify, Administration, Test, Devtools. The "Wallet" group is now "Administration" and also holds Keys and Certificates, "Wallet Settings" is now "Settings", and "Admin Accounts" is now "Accounts". Links to external sites (Documentation, Helpdesk) show an external-link icon.
- **SVX Verify settings in one place:** SVX Verify "Configuration" is now "Settings" (`/svx-verify/settings`; the old URL redirects). The SVX Verify session policy (response mode, session expiry, multiple request loads) and Bridge integration settings moved there from Settings › Verifier. The "SVX Verify setup is incomplete" checklist shows on this page, the sidebar entry shows an amber marker while setup is incomplete, and missing required fields are marked as soon as the page loads.
- **Verifier settings no longer blocked:** an incomplete SVX Verify setup no longer prevents saving verifier settings (`PUT /system/settings/verifier`), so other verification flows can be configured while SVX Verify is still being set up.
- **Unsaved changes:** Administration › Settings and SVX Verify › Settings ask before discarding unsaved changes when you navigate away, and the browser asks before reloading or closing the tab. The save-conflict dialog no longer offers "Keep my changes", which discarded the edits anyway: Cancel returns to the form with your edits, and OK loads the latest settings.
- **Bridge settings:** the Connect ID and Select ID sections are disabled, with a "Disabled in server configuration" warning, while the integration is not enabled in `config.json`.
- **mdoc templates:** "No expiry" is disabled for `mso_mdoc` credential templates, and expiry dates later than the signing certificate's `notAfter` are blocked.
- **Forms:** required fields are marked with `*`, restricted fields show allowed characters and maximum length, and errors follow the SVX Design System. Edit forms consistently use a "Save" button.
- **Consistency:** credential formats are shown everywhere as "IETF SD-JWT VC", "ISO mdoc" and "W3C VCDM (JWT)"; navigation labels and page headings use Title Case; date columns are named consistently (e.g. "Date Issued", "Date Created"); empty values show an em dash; and long identifiers can be copied to the clipboard.
- **Deep links:** credential schema and verification template detail panels have their own URLs (`/credentials/schemas/:id`, `/verification/templates/:id`), so they survive a page refresh and can be shared.
- **Tables:** list tables keep stable column widths across pages, truncate overflowing text, and scroll horizontally on narrow screens.
- **Verification requests:** the request history on the Verification Request detail page has a working refresh control. Verification templates of type `device_request` are hidden from the Test Verify and SVX Verify template pickers.
- **Look and feel:** the empty states, footer ("Powered by Meeco") and credential template preview card use the official SVX Design System assets, and the demo Issue and Verify page copy has been reworded for clarity.

### Other improvements

- The verification success screen button now reads "Continue" instead of "Continue to booking", in all locales.
- Credential configuration is now cached in memory by each instance instead of in Redis. The new runtime setting `issuer.credential_configuration_cache_ttl` (seconds, default `300`) bounds how long a cached configuration is used if no change notification arrives. Saving a credential template now refreshes the Test Issue page immediately.
- DCQL credential query ids may now contain `.`.
- Encrypted presentation responses: when a presentation request's `client_metadata.jwks` contains several keys, the wallet now uses the first key it supports for response encryption (`ECDH-ES` with an EC or OKP key on P-256, P-384, P-521 or X25519, and not restricted by `use` or `key_ops` to a purpose other than encryption) instead of always the first key. Previously the response was always encrypted with a P-256 ephemeral key; it now uses the curve of the selected key. Requests whose `jwks` contains no supported key, or whose `encrypted_response_enc_values_supported` contains no supported value, are rejected with a message listing what is supported.
- `POST /system/certificates/import` rejects `x5c` items that are not valid certificates with `400 invalid_request` (with `extra_info.details.x5c_index`) instead of a `500`.

## Security

- Fixed `did:jwk` DIDs created by the wallet (`POST /wallets/{id}/dids`) containing the private key. They were built from the full generated JWK, so the private key (`d`) was embedded in the DID and in everything derived from it, such as key IDs, proof JWTs, issued credentials and presentations. Only the public JWK is encoded now. **DIDs created before this release are not updated and still contain the private key; they should be replaced.**
- Upgraded `@meeco/sd-jwt` to 1.3.1 and `@meeco/sd-jwt-vc` to 2.4.0, which verify SD-JWT presentations more strictly: the Key Binding JWT's `typ`, `sd_hash` and `iat` are now checked.
- The production images no longer contain application source code or source maps, and the backend is bundled and obfuscated.
- The Admin UI and server-rendered pages no longer load fonts from Google: fonts are self-hosted or use the system font stack, and `fonts.googleapis.com` and `fonts.gstatic.com` have been removed from the Content Security Policy.
- Upgraded Node.js to 24.22.1 (security release) and resolved all known vulnerabilities in the production dependencies of the SVX Platform.
- Reduced known vulnerabilities in the SVX Portal's dependencies, with no critical findings remaining.

## Bug Fixes

- Fixed OpenID4VP verification accepting presentations without holder binding when the DCQL query required it (see [Holder binding enforcement and DCQL error reporting](#verification-holder-binding-enforcement-and-dcql-error-reporting)).
- Fixed roughly 1 in 8 P-256 keys generated by the wallet having a JWK, and a `did:jwk`, that did not match the key the wallet signs with: coordinates starting with a zero byte were shortened instead of encoded as 32 bytes.
- Fixed `POST /wallets/{id}/send/submit` returning a `500` when no credentials were selected; it now returns `400 no_credentials_selected`, and nothing is sent to the verifier.
- Fixed `POST /wallets/{walletId}/credentials/import` rejecting the `meta.kid` and `meta.did` fields described in the API specification.
- Fixed two credential templates with the same custom credential configuration id replacing each other in the issuer metadata, which left one of them unusable for issuance.
- Fixed `schema.schema_url` in credential template responses pointing to a URL that returned `404`; it now points to `/issuer/schemas/{id}/schema.json`.
- Fixed `background_image` in the issuer credential metadata; it is now an object of the form `{ "url": "..." }`.
- Fixed `GET /.well-known/openid-credential-issuer` not including each credential's configured validity (`valid_at`, `valid_in`, `expires_at`, `expires_in`) in `credential_metadata.display`.
- Fixed `POST /system/certificates/csrs` truncating values or failing with a `500` when subject fields contained commas, `+` or `=`. Subject attributes are now ordered C, ST, L, O, OU, CN.
- Fixed the "At Issuance" valid-from option in the credential template form, which made credentials valid from the time the template was saved rather than the time of issuance. Reopening such a template no longer turns it into a specific date and time.
- Fixed several W3C Bitstring Status List issues: a race when allocating status list indexes and the reuse of deleted indexes, the bit order (now most-significant bit first, as W3C requires), and status entries created already revoked not being reflected in the published list. Verification now fails on an invalid or out-of-range `statusListIndex`, a status purpose that does not match the credential entry, or a `statusSize` the verifier cannot read.
- Fixed the SVX Verify PDF receipt showing the hardcoded "Express booking and check-in." instead of the configured SVX Verify title.
- Fixed the SelectID relying party metadata `client_uri`, which pointed to a non-existent page.
- Fixed editing a credential schema in the Admin UI not re-checking the JSON against its format; Save is disabled while it doesn't match, and server-side validation errors are listed.
- Fixed Admin UI credential templates requiring a type for `jwt_vc_json`, which blocked saving templates created with only `VerifiableCredential`.
- Fixed Admin UI credential template colour fields rejecting 3-digit hex colours (`#rgb`).
- Fixed leading and trailing whitespace in credential schema and template names, which allowed duplicate-looking names.
- Fixed API key names longer than 100 characters being truncated in the dashboard, and long names pushing the Delete button outside the confirmation dialog.
- Fixed the Test Issue page showing `[object Object]` instead of the validation error, pre-filling every claim with an empty string regardless of its type, and showing a made-up validity date for templates without a valid-from date.
- Fixed inconsistent fonts across the server-rendered pages.

## Other Components

### SVX Portal

- After creating an organisation or tenant, the Portal now opens its dashboard instead of returning to the list.
- Fixed a spurious "Leave site?" prompt when refreshing or closing a page after viewing an application. The client secret is no longer stored in the browser's local storage.
- Upgraded Node.js to 24.22.0 and TypeScript to 6.0.3, along with Vite, Storybook, react-router-dom, ajv and other dependencies.

### Self-service wallet deployments

- Organisation wallets deployed from the Portal are now uninstalled automatically when their organisation or tenant is archived or deleted.
- For self-hosted platform deployments, SVX Utilities has two new mandatory configuration fields: `auto_deployments.process_atom_jobs` enables or disables this processing, and `postgres.direct_connection_port_for_listen_notify` points to a direct, non-pooled PostgreSQL port (connection poolers such as PgBouncer don't relay LISTEN/NOTIFY reliably). `GET /system/status` reports the queue state under `job_queues.atom_to_svx_utilities_queue`.

### Platform reliability

- ATOM handlers and projectors that crash are now restarted automatically, retry failed event store subscriptions with exponential backoff, and re-register after a registry restart.
- Fixed commands dispatched with strong consistency returning stale data when no handler was registered for their event type.
- ATOM can now run as multiple full-mode instances in a cluster. The `presence` key has been removed from ATOM's `GET /system/status`.

## Changed

- Upgraded the SVX Wallet Gateway and SVX Gateway to KrakenD 2.13.10 and 2.13.11 respectively. The gateway now serves `/openapi` and `/openapi/ui` from SVX Wallet.
- Upgraded ATOM and SVX Utilities to Erlang 29.1.2, and SVX Utilities to Elixir 1.20.4.
- Upgraded the SVX Platform to Node.js 24.22.1 and `@nestjs/throttler` to 6.7.1, along with minor and patch upgrades of other dependencies.
