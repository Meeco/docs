# SVX 4.3.0 Release Notes

**Software Release Date:** TBD

**Summary:**

This is a feature release for the SVX Platform. On the wallet service it adds:

- A **credential catalog** for installing standard credentials (mDL, EU PID, Age Verification and more) in one step.
- **IETF Token Status List** support, so credentials in every format can be revoked and suspended, with status lists cached by the verifier.
- **Keys and Certificates** management in the Dashboard and API, including certificate import and activation.
- An **ISO 18013-5 Reader CA** for issuing reader authentication certificates.
- **Import of ISO 18013-5 verifications** performed outside SVX.
- A new **`device_request`** verification template type.
- **Search** across the main Dashboard tables.

It also consolidates SVX Verify configuration into a single Settings page, enforces DCQL cryptographic holder binding correctly, and includes a broad round of Dashboard consistency work, bug fixes and security hardening. The API gateway, SVX Utilities and Meeco Portal gain supporting changes.

⚠️ **This release contains breaking changes.** Most importantly for OIDC deployments, admin authentication moves to `config.json` and must be set there before upgrading. Review [Breaking Changes](#breaking-changes) before upgrading.

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

**In the Dashboard:** the new Credentials › Catalog page, also reachable with **Add from catalog** on the Credential Templates page, installs catalog entries. The install sheet shows the schema and templates already installed for the chosen format, pre-selects only what is missing, and prevents installing a part again under the name of an existing template. On the Test Issue page, claims can be filled in from one of the samples of the template's catalog entry.

The catalog can be hidden with the runtime setting `system.credential_catalog.visible` (default `true`, Administration › Settings › System › Credential catalog). When hidden, the Dashboard removes the Catalog page and **Add from catalog**, and the `/catalog` endpoints return `404 catalog_not_visible`. Templates already installed from the catalog keep working.

### Status lists: IETF Token Status List support

SVX now supports the IETF Token Status List ([draft-ietf-oauth-status-list](https://datatracker.ietf.org/doc/draft-ietf-oauth-status-list/)) alongside the W3C Bitstring Status List, so `dc+sd-jwt` and `mso_mdoc` credentials can now be revoked and suspended.

**Choosing a status list:** each credential template selects its status list with `status_list`:

- `ietf_list`: available for all formats; the default for `dc+sd-jwt` and `mso_mdoc`.
- `w3c_list`: `jwt_vc_json` only; the default for `jwt_vc_json`.
- `none`: credentials are not revocable.

The setting is available in the Dashboard template forms. It replaces `issuer.issuance_policy.revocable`, which is now deprecated and ignored.

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

New runtime settings, also editable in the Dashboard Settings:

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

### Keys and Certificates management

- `GET /system/keys` lists the deployment's KMS keys, including keys retired by rotation: algorithm, purpose, adapter, key ID, public JWK, use count, bound certificates and creation/retirement dates. A new read-only **Administration › Keys** page gives an overview with a detail panel (bound certificates, previous versions) and a copy-as-JWKS action.
- Certificate responses (`GET /system/certificates`, `GET /system/certificates/{id}`, `POST /system/certificates/import`) now include a parsed `chain`: subject, issuer, serial, validity, public key, SANs, CA/self-signed flags and SHA-256 fingerprint. The list can be filtered by `type`, `status` and `key_name`.
- New read-only **Administration › Certificates** page lists managed, trust-anchor and IACA certificates with the full chain, copy-as-PEM actions and links to the bound key. Certificates can be **imported** (trust anchors, or managed chains bound to a signing key) and **activated/deactivated** from the UI and API (`POST /system/certificates/{id}/activate` / `/deactivate`). A key may hold several certificates but only one active.

**Certificate activation and renewal:** the active certificate is the one attached as `x5c` to what the key signs. The first certificate imported for a key is active on import; later ones, such as a renewal, are imported inactive. This lets you import a renewed certificate ahead of time and activate it when needed:

```bash
curl -sS -X POST "$SVX_WALLET_BASE_URL/system/certificates/$CERTIFICATE_ID/activate" \
  -H "Authorization: Bearer $ACCESS_TOKEN" | jq '.certificate | {id, active, activated_at}'
```

Activating a certificate deactivates the key's previously active one in the same step. Only `managed` certificates can be activated (`certificate_activation_not_supported` otherwise), and not when their key version has been retired by rotation (`certificate_key_retired`). Certificate responses include `active`, `activated_at` and `deactivated_at`.

![Administration › Keys](/.gitbook/assets/Release_4.3.0_Keys.png)

![Administration › Certificates](/.gitbook/assets/Release_4.3.0_Certificates.png)

### ISO 18013-5 Reader CA

- New runtime setting `verifier.reader_ca.enabled` (requires `verifier.enabled`) and an optional ES256 managed key `ReaderCAKey`. The issuing CA certificate is signed outside SVX and imported via `POST /system/certificates/import` (strict validation of CA certificates; others rejected with `invalid_reader_ca_certificate`).
- New endpoints (enabled only when the setting is on):
  - `POST /verifier/reader/certify` — issues reader authentication certificates for a batch of CSRs, signed by `ReaderCAKey` (configurable leaf validity and batch size). Leaf validity is set by `verifier.reader_ca.leaf_validity_hours` (default `24`) and batch size by `verifier.reader_ca.max_csrs_per_request` (default `10`). The certificate's CN comes from the CSR, and CSRs without one are rejected; C, O and OU come from the issuing CA. Each result, in request order, is either `{ "x5c": [reader certificate, issuing CA, ...] }` or a per-CSR `{ "error": ... }`.
  - `GET /verifier/reader/issuing-ca.pem` / `.der` — publishes the issuing CA certificate for wallets to trust, with an `X-Certificate-Fingerprint-SHA256` header.
  - `GET /verifier/reader/crl` — publishes a signed (empty) CRL referenced by every reader certificate.
- Dashboard: a **Reader authentication** section under Settings › Verifier (enable, certificate lifetime, certificates per request) showing the active signing certificate. New metric `reader_ca_certifications_total{result}`.

![Reader authentication settings](/.gitbook/assets/Release_4.3.0_Reader_Authentication.png)

### ISO 18013-5 verification import

`POST /verifier/requests/import` imports ISO 18013-5 verifications performed outside SVX, for example by mdoc readers, so they can be stored and reviewed alongside verifications made through SVX. Each record contains `record_reference_id`, `device_request`, `device_response`, `requested_at`, the reported `verification_result` and an optional `session_transcript`, and is stored as an imported presentation request and response with protocol `iso_mdoc`. The verification result is reported by the uploader and is not re-verified by SVX.

- A record without `device_response` and `verification_result` but with a past `expires_at` represents a request that expired unanswered. It is stored as a `pending` imported request without a response, like an expired OpenID4VP request.
- Imports are idempotent on `record_reference_id`, and stored records are never updated. A different record under an existing `record_reference_id` is rejected as `reference_conflict` with the id of the stored record; to replace it, delete it and import again.
- The runtime setting `verifier.request_import.max_records_per_request` (default `100`) caps the number of records per request. It is editable in the new Request import section of Administration › Settings › Verifier.

Presentation requests now have a `protocol` (`openid4vp` or `iso_mdoc`) and an `imported` flag, and `iso_mdoc` requests carry `record_reference_id` and `doc_requests`. Presentation responses gain `device_response` and `session_transcript`. Verification results gain an optional `device_response` part for ISO 18013-5 DeviceResponses (response-level `checks`, `warnings` and `errors`, and `documents` with a `doc_type` each), which holds the reported result of an imported request. `GET /verifier/requests/{id}/jwt` and `POST /verifier/requests/{id}/responses` return `404 not_found` for `iso_mdoc` requests.

### `device_request` verification templates

- New verification template type storing `doc_requests`, the JSON equivalent of ISO 18013-5 DocRequests, with an IntentToRetain flag per data element. These templates cannot be used to create presentation requests. The Dashboard JSON editor offers a Device Request starter and an mDL sample with structure checks, and hides these templates from the Test Verify and SVX Verify pickers.

![Device Request starter in the verification template JSON editor](/.gitbook/assets/Release_4.3.0_Device_Request_Template.png)

### Dashboard search

- Search boxes on the View Credentials, Verification Requests, API Keys and Administrator Accounts pages. The matching list endpoints accept a `search` query param (max 50 chars) matching name or ID case-insensitively; verifier request search also matches the request ID.

![Search on the View Credentials page](/.gitbook/assets/Release_4.3.0_Credential_Search.png)

### Issuance

- Ad-hoc issuance (`POST /issuer/credentials`) accepts optional `valid_at` / `expires_at` (ISO 8601) to override the template's validity for that issuance, across `dc+sd-jwt`, `jwt_vc_json` and `mso_mdoc`. They set `nbf` / `exp` for `dc+sd-jwt` and `jwt_vc_json`, and `ValidityInfo.validFrom` / `validUntil` for `mso_mdoc`. Either can be given on its own; the side that isn't set still comes from the template. Requests whose resulting validity ends before it starts, or has already ended, are rejected with `invalid_request`. Responses now include the issuer credential `id` for each credential (`credentials: [{ id, credential }]`), so it can be used directly with endpoints such as `PATCH /issuer/credentials/{id}/status`.
- `POST /issuer/offers` (and `POST /demo/issuer/offers`) accept an optional `credential_offer_endpoint` that overrides the configured endpoint for that offer only, with a matching field on the Test Issue page.

### DC API credential issuance

- SVX can now issue credentials over the browser's **W3C Digital Credentials API** (experimental). The credential offer registered with the existing `POST /wallets/{walletId}/receive` endpoint can now include `credential_issuer_metadata` and `authorization_server_metadata` inline. When present, they are used instead of `.well-known` discovery; this is required for offers delivered through the DC API, because the wallet-selection matcher has no network access.
- The demo Issue page offers a DC API option when the browser supports it, and shows errors the browser reports. In cross-device flows the browser can report an error even when issuance succeeded, so check the wallet; the same-device flow works as expected.

### Other additions

- Admin display names can be edited via `PATCH /admin/accounts/{id}` and the Dashboard; passkeys show the display name instead of a UUID.
- Test Verify page: exclude `state` from the request, set a verifier Redirect URI, and set an optional Request Name.
- `POST /verifier/requests/{id}/responses` accepts an optional `options.expected_origin` used as the `origin:` audience and `mso_mdoc` session-transcript origin for DC API response modes.
- SVX Verify receipts can show an optional HTTPS `contact_url`.
- `GET /wallets/{walletId}/send` filters presentation states by submission status with the `status` query param (`pending`, `failed`, `success`), as a comma-separated list or a repeated param; other values are rejected with `invalid_status_filter_value`.
- Devtools › API Documentation opens the Swagger UI in a new tab when `system.swagger_ui_enabled` is set.

## Breaking Changes

- **Admin authentication is now static config.** `system.admin.auth_method` / `system.admin.idp` move from runtime settings to `config.json` and are no longer editable in the Dashboard (`auth_method` defaults to `passkey`). A migration removes these keys from stored settings — **OIDC deployments must set them in `config.json` before upgrading** (and remove `admin` from Helm `runtimeBootstrap.systemDocument`).
- **DCQL holder binding is now enforced.** OpenID4VP verification previously accepted every presentation regardless of the DCQL query's `require_cryptographic_holder_binding`, and a query that omitted the flag was treated as not requiring binding. The specification default (`true`) is now applied, and binding is derived from the presentation itself: a Key Binding JWT verified against the credential's `cnf.jwk` for `dc+sd-jwt`, a holder key or subject DID tied to the presentation signer for `jwt_vc_json`, and device authentication for `mso_mdoc`. **Presentations without holder binding are now rejected unless the credential query sets `"require_cryptographic_holder_binding": false`.** Verification results record a new `holder_binding` check. On the wallet side, `set_credentials` returns an error when a selected credential cannot satisfy the requirement.
- **DCQL verification errors are more specific.** Instead of a single `failed_to_validate_presentation_response`, DCQL failures are reported as a distinct error per kind of mismatch, each listing the affected credential queries: `dcql_presentation_missing_cryptographic_holder_binding`, `dcql_presentation_does_not_match_credential_metadata`, `dcql_presentation_claims_do_not_satisfy_query`, `dcql_presentation_issuer_is_not_a_trusted_authority`, `dcql_presentation_missing_for_credential_query` and `dcql_required_credential_set_not_satisfied`.
- **DC API response origin.** Authorization response submission no longer derives the Digital Credentials API origin from request headers. It comes from `options.expected_origin` on `POST /verifier/requests/{id}/responses`, falling back to `system.app_host`.
- **SVX Verify sessions require a complete setup.** `POST /verify/sessions` returns `409 svx_verify_not_configured`, with the missing fields in `extra_info.missing_fields`, until the SVX Verify brand name, operating company, terms URL, privacy URL and at least one identity provider are set.
- **Admin display names are required and unique.** `POST /admin/invitations` now requires `display_name`. A migration names unnamed admins `Admin` and suffixes duplicates (` (2)`, ` (3)`).
- **Managed certificates cannot be deleted once they have been active.** `DELETE /system/certificates/{certificate_id}` returns `409 certificate_has_been_active`; deactivate them instead. A migration marks the newest existing certificate per key as active, so all existing managed certificates count as having been active.
- **ConnectID must be explicitly enabled.** ConnectID now only runs when `bridge.integrations.connect_id.enabled` is `true`, as SelectID already did. The field is required, so existing `connect_id` blocks in `config.json` need `"enabled": true`. Setting it to `false` while runtime ConnectID settings are saved prevents the server from starting.
- **`mso_mdoc` credential templates must have an expiry.** Creating or updating an mdoc template without `expires_at` or `expires_in` fails with `validation_error`, and issuing from an existing mdoc template without one fails with `mso_mdoc_expiry_required` instead of falling back to a far-future date. The expiry also cannot be later than the signing (DS) certificate's `notAfter`; issuance fails with `mso_mdoc_expiry_exceeds_certificate` otherwise. "No expiry" is disabled for mdoc templates in the Dashboard.
- **OpenAPI document and Swagger UI moved.** The OpenAPI document is served at `GET /openapi` and the Swagger UI at `/openapi/ui` (previously `/api-json` and `/api`). The document is always served; `system.swagger_ui_enabled` now controls only the UI.
- **Presentation request fields are nullable.** To support [imported ISO 18013-5 verifications](#iso-18013-5-verification-import), `response_type`, `response_mode`, `nonce` and `expires_at` can now be `null` in `GET /verifier/requests` and `GET /verifier/requests/{id}`. This is a type change in the generated SDKs.
- **Issuer-controlled claims cannot be supplied.** Claims the issuer sets itself (for example `nbf`, `exp`, `iat`, `iss`, `status` or `credentialStatus`) are rejected with a `400` naming the claim when supplied in offers or ad-hoc issuance. `jwt_vc_json` and `dc+sd-jwt` schemas can no longer define them, and claims with these names from resource hooks and claim mappings are ignored.
- **Name uniqueness.** Credential schema and credential template names are now unique per format, and verification template names unique per type (`pex`, `dcql`, `device_request`), rather than globally. A custom credential configuration id can only be used by one credential template (see [Bug Fixes](#bug-fixes)). API key names are limited to 100 characters (previously 255).
- **`issuer.issuance_policy.revocable` is deprecated and ignored**, replaced by the per-template `status_list` setting. See [IETF Token Status List support](#status-lists-ietf-token-status-list-support).

## Changed

- **Dashboard navigation redesign:** SVX Design System icons, reordered sections (Dashboard, Credentials, SVX Verify, Administration, Test, Devtools), "Admin Accounts" → "Accounts", and Keys/Certificates added under Administration.
- **SVX Verify configuration consolidated:** session policy and Bridge integration moved from Settings › Verifier onto a single SVX Verify **Settings** page (`/svx-verify/configuration` redirects to `/svx-verify/settings`); the setup checklist and an amber "incomplete" marker live there. An incomplete SVX Verify setup no longer blocks saving verifier settings.
- SVX Verify contact **phone and email are now optional** (receipt shows a localised placeholder).
- Credential configuration is cached in-memory per instance instead of Redis, bounded by a new `issuer.credential_configuration_cache_ttl` setting (default 300s).
- Public SVX Verify and authorization pages now send a CSP `form-action` directive (configurable via `system.security.headers.content_security_policy.form_action`); known redirect targets are allowed automatically.
- Dashboard forms mark required fields with `*` and show allowed-character / max-length hints. The verification success screen button now reads "Continue".
- Administration › Settings and SVX Verify › Settings ask before discarding unsaved changes when navigating away, and the browser asks before reloading or closing the tab. The save-conflict dialog no longer offers "Keep my changes", which discarded the edits anyway: Cancel returns to the form with the edits kept, OK loads the latest settings.
- The Dashboard Bridge settings disable the Connect ID and Select ID sections, with a "Disabled in server configuration" warning, while the integration isn't enabled in `config.json`.
- Encrypted presentation responses: when a request's `client_metadata.jwks` contains several keys, the wallet uses the first key it supports (`ECDH-ES` with an EC or OKP key on P-256, P-384, P-521 or X25519) instead of always the first key, and generates its ephemeral key on that key's curve instead of always P-256. Requests with no supported key are rejected with a message listing what is supported.
- DCQL credential query ids may now contain `.`.

## Bug Fixes

- Credential Template previews and the Test Issue page no longer show a fabricated "today" validity and now reflect the template's actual saved validity. Issuer metadata `display` returns the template's real validity fields.
- `POST /system/certificates/csrs` builds the CSR subject from its fields (keeps `,`, `+`, `=` in values); `POST /system/certificates/import` rejects invalid `x5c` items with `400` instead of `500`.
- Test Issue page surfaces the real validation error (instead of `[object Object]`) and seeds placeholder claims with type-correct defaults. Color fields accept 3-digit hex. `POST /:walletId/credentials/import` now whitelists `meta.kid` / `meta.did`. SelectID `client_uri` points to `/verify/information`.
- P-256 keys generated by the wallet encode their JWK `x` and `y` as 32 bytes. A coordinate starting with a zero byte was shortened, so roughly 1 in 8 keys had a JWK, and a `did:jwk`, that did not match the key the wallet signs with.
- `POST /wallets/{id}/send/submit` with no credentials selected returns `400 no_credentials_selected` instead of a `500`; nothing is sent to the verifier.
- **W3C Bitstring Status List:** fixed a race when allocating indexes and the reuse of deleted indexes, the bit order (now most-significant bit first, as W3C requires), and status entries created already revoked not being reflected in the published list. Verification now fails on an invalid or out-of-range `statusListIndex`, a status purpose that doesn't match the credential entry, or a `statusSize` the verifier cannot read.
- `schema.schema_url` in credential template responses points to `/issuer/schemas/{id}/schema.json`; it pointed to `/schemas/{id}/schema.json`, which returned 404.
- The credential template "At Issuance" valid-from option no longer sets `valid_at` to the time the template was saved, so credentials are valid from the moment they are issued. Reopening such a template no longer turns it into a specific date and time.
- Issuer credential metadata `background_image` is now an object of the form `{ "url": "..." }`.
- A custom credential configuration id (`config.issuer_credential_configurations_supported_name`) can only be used by one credential template: a second template with the same id replaced the first in the issuer metadata, so one of them could no longer be issued. Duplicates are now rejected with `credential_template_configuration_id_already_exist`, and a migration renames existing ones, keeping the id of the oldest template and adding the format suffix (for example `_mso_mdoc`) to the others.
- Dashboard credential templates: Type is now optional for `jwt_vc_json`, as in the API, so templates created with only `VerifiableCredential` can be edited and saved.
- Dashboard credential schemas: editing a schema re-checks the JSON against its format as you type and disables Save while it doesn't match; server-side validation errors are listed in the error toast.
- **Dashboard consistency:** consistent human-readable credential-format labels, "Date …" column headings, copyable long identifiers, a shared em dash for empty values, Title Case labels, a single page description per page, stable table column widths (with horizontal scroll on narrow viewports), routed detail drawers that survive refresh, a working request-history refresh control, the official empty-state icon and "Powered by Meeco" footer, trimmed schema/template names, and clearer demo Issue/Verify copy.
- **Meeco Portal:** creating an organization or tenant now redirects to the new entity's dashboard instead of the list page; viewing an Application no longer shows a spurious "Leave site?" prompt when the page is refreshed or closed.

## Platform and infrastructure

- **API gateway:** the gateway now proxies the new wallet endpoints:
  - Catalog: `GET /catalog`, `GET /catalog/{id}`, `POST /catalog/{id}/install`, `GET /issuer/templates/{id}/samples` and the public `GET /demo/issuer/templates/{id}/samples`.
  - Keys and certificates: `GET /system/keys`, `GET /system/certificates/{id}`, `POST /system/certificates/{id}/activate` and `/deactivate`.
  - ISO 18013-5: `POST /verifier/requests/import`, `POST /verifier/reader/certify`, and the public `GET /verifier/reader/issuing-ca.pem`, `GET /verifier/reader/issuing-ca.der` and `GET /verifier/reader/crl`.
  - Other: `GET /issuer/status_lists` and `PATCH /admin/accounts/{id}`.
  - `/openapi` and `/openapi/ui` are passed through to SVX Wallet.
- **SVX Utilities:** now automatically uninstalls an organization's or tenant's wallets when it is archived or deleted, so wallets are cleaned up without manual intervention. Two new **mandatory** config fields: `auto_deployments.process_atom_jobs` enables or disables this processing, and `postgres.direct_connection_port_for_listen_notify` points to a direct, non-pooled PostgreSQL port for the queue's LISTEN/NOTIFY connection, since connection poolers such as PgBouncer don't relay it reliably. `GET /system/status` reports the queue state under `job_queues.atom_to_svx_utilities_queue`.
- **ATOM:** crashed handlers and projectors are restarted automatically, retry failed event store subscriptions with exponential backoff, and re-register after a registry restart. Commands dispatched with strong consistency no longer return stale data when no handler is registered for their event type. ATOM can now run as multiple full-mode instances in a cluster, and the `presence` key has been removed from its `GET /system/status`.
- **Runtime upgrades:** SVX Platform to Node.js 24.22.1, SVX Wallet Gateway and SVX Gateway to KrakenD 2.13.10 and 2.13.11, ATOM and SVX Utilities to Erlang 29.1.2, and SVX Utilities to Elixir 1.20.4.
- **Meeco Portal:** upgraded to Node.js 24 and TypeScript 6 (with updated ESLint tooling).

## Security

- Fixed `did:jwk` DIDs created by the wallet (`POST /wallets/{id}/dids`) containing the private key. They were built from the full generated JWK, so the private key (`d`) was embedded in the DID and in everything derived from it, such as key IDs, proof JWTs, issued credentials and presentations. Only the public JWK is encoded now. **DIDs created before this release are not updated and still contain the private key.**
- The production images of SVX Wallet and SVX Platform no longer contain application source code or source maps: the backend is bundled and obfuscated.
- SVX Platform upgraded to Node.js 24.22.1 (a security release), and all known vulnerabilities in its production dependencies, including critical ones, have been resolved. DM Sans is now self-hosted: the Content Security Policy no longer allows `fonts.googleapis.com` / `fonts.gstatic.com`, and server-rendered pages use a system sans-serif stack.
- Upgraded `@meeco/sd-jwt` (1.3.1) and `@meeco/sd-jwt-vc` (2.4.0): SD-JWT presentation verification now checks the Key Binding JWT's `typ`, `sd_hash` and `iat` in the library.
- **Meeco Portal:** the application Client Secret is no longer stored in `localStorage`, and dependency vulnerabilities were reduced by upgrading 31 vulnerable packages.
