# SVX 4.3.0 Release Notes

**Software Release Date:** TBD

**Summary:**

This is a feature release for the SVX Platform. It adds read-only **Keys and Certificates** management to the Admin UI and API, an **ISO 18013-5 Reader CA** for issuing reader authentication certificates, a new **`device_request`** verification template type, and **search** across the main dashboard tables. It also consolidates SVX Verify configuration into a single Settings page, enforces DCQL cryptographic holder binding correctly, and includes a broad round of Admin UI consistency work, bug fixes and security hardening.

## New Features

### Keys and Certificates management

- `GET /system/keys` lists the deployment's KMS keys, including keys retired by rotation: algorithm, purpose, adapter, key ID, public JWK, use count, bound certificates and creation/retirement dates. A new read-only **Setup › Keys** page gives an overview with a detail panel (bound certificates, previous versions) and a copy-as-JWKS action.
- Certificate responses (`GET /system/certificates`, `GET /system/certificates/{id}`, `POST /system/certificates/import`) now include a parsed `chain`: subject, issuer, serial, validity, public key, SANs, CA/self-signed flags and SHA-256 fingerprint. The list can be filtered by `type`, `status` and `key_name`.
- New read-only **Setup › Certificates** page lists managed, trust-anchor and IACA certificates with the full chain, copy-as-PEM actions and links to the bound key. Certificates can be **imported** (trust anchors, or managed chains bound to a signing key) and **activated/deactivated** from the UI and API (`POST /system/certificates/{id}/activate` / `/deactivate`). A key may hold several certificates but only one active.

![Setup › Keys](/.gitbook/assets/Release_4.3.0_Keys.png)

![Setup › Certificates](/.gitbook/assets/Release_4.3.0_Certificates.png)

### ISO 18013-5 Reader CA

- New runtime setting `verifier.reader_ca.enabled` (requires `verifier.enabled`) and an optional ES256 managed key `ReaderCAKey`. The issuing CA certificate is signed outside SVX and imported via `POST /system/certificates/import` (strict validation of CA certificates; others rejected with `invalid_reader_ca_certificate`).
- New endpoints (enabled only when the setting is on):
  - `POST /verifier/reader/certify` — issues reader authentication certificates for a batch of CSRs, signed by `ReaderCAKey` (configurable leaf validity and batch size).
  - `GET /verifier/reader/issuing-ca.pem` / `.der` — publishes the issuing CA certificate for wallets to trust.
  - `GET /verifier/reader/crl` — publishes a signed (empty) CRL referenced by every reader certificate.
- Admin UI: a **Reader authentication** section under Settings › Verifier (enable, certificate lifetime, certificates per request) showing the active signing certificate. New metric `reader_ca_certifications_total{result}`. See `docs/reader-ca-configuration.md`.

![Reader authentication settings](/.gitbook/assets/Release_4.3.0_Reader_Authentication.png)

### `device_request` verification templates

- New verification template type storing `doc_requests`, the JSON (snake_case) equivalent of ISO 18013-5 DocRequests, with an IntentToRetain flag per data element. These templates cannot be used to create presentation requests. The dashboard JSON editor offers a Device Request starter and an mDL sample with structure checks, and hides these templates from the Test Verify and SVX Verify pickers.

![Device Request starter in the verification template JSON editor](/.gitbook/assets/Release_4.3.0_Device_Request_Template.png)

### Dashboard search

- Search boxes on the View Credentials, Verification Requests, API Keys and Administrator Accounts pages. The matching list endpoints accept a `search` query param (max 50 chars) matching name or ID case-insensitively; verifier request search also matches the request ID. Trigram indexes keep credential search fast as the table grows.

![Search on the View Credentials page](/.gitbook/assets/Release_4.3.0_Credential_Search.png)

### Issuance

- Ad-hoc issuance (`POST /issuer/credentials`) accepts optional `valid_at` / `expires_at` (ISO 8601) to override the template's validity for that issuance, across `dc+sd-jwt`, `jwt_vc_json` and `mso_mdoc`. Responses now include the issuer credential `id` for each credential.
- `POST /issuer/offers` (and the demo endpoint) accept an optional `credential_offer_endpoint` that overrides the configured endpoint for that offer only, with a matching field on the Test Issue page.

### Other additions

- Admin display names can be edited via `PATCH /admin/accounts/{id}` and the Admin UI; passkeys show the display name instead of a UUID.
- Test Verify page: exclude `state` from the request, set a verifier Redirect URI, and set an optional Request Name.
- Demo UI gains a DC API issuance option when the browser supports it.
- `POST /verifier/requests/{id}/responses` accepts an optional `options.expected_origin` used as the `origin:` audience and `mso_mdoc` session-transcript origin for DC API response modes.

## Changed

- **Admin authentication is now static config.** `system.admin.auth_method` / `system.admin.idp` move from runtime settings to `config.json` and are no longer editable in the dashboard (`auth_method` defaults to `passkey`). A migration removes these keys from stored settings — **OIDC deployments must set them in `config.json` before upgrading** (and remove `admin` from Helm `runtimeBootstrap.systemDocument`).
- **Dashboard navigation redesign:** SVX Design System icons, reordered sections (Dashboard, Credentials, SVX Verify, Administration, Test, Devtools), "Admin Accounts" → "Accounts", and Keys/Certificates added under Administration.
- **SVX Verify configuration consolidated:** session policy and Bridge integration moved from Settings › Verifier onto a single SVX Verify **Settings** page (`/svx-verify/configuration` redirects to `/svx-verify/settings`); the setup checklist and an amber "incomplete" marker live there. An incomplete SVX Verify setup no longer blocks saving verifier settings; `POST /verify/sessions` returns `409 svx_verify_not_configured` with the missing fields until setup is complete.
- SVX Verify contact **phone and email are now optional** (receipt shows a localised placeholder), and an optional HTTPS `contact_url` is shown on the receipt.
- Credential schema names are now unique **per format** rather than globally.
- Admin display names are now **required** (`POST /admin/invitations`) and **unique** (migration renames/suffixes duplicates).
- **W3C Bitstring Status List** rework: larger lists, multibase base64url encoding, string `statusListIndex`, randomly allocated entries, and a dedicated `StatusListKey` signing key.
- Credential configuration is cached in-memory per instance instead of Redis, bounded by a new `issuer.credential_configuration_cache_ttl` setting (default 300s). Settings and credential-configuration change notifications are now raised by PostgreSQL triggers.
- Public SVX Verify and authorization pages now send a CSP `form-action` directive (configurable via `system.security.headers.content_security_policy.form_action`); known redirect targets are allowed automatically.
- OpenID4VP credential selection honours the DCQL `require_cryptographic_holder_binding` requirement. DC API response origin now comes from `options.expected_origin` (falling back to `system.app_host`) rather than request headers.
- Admin UI forms mark required fields with `*` and show allowed-character / max-length hints. The verification success screen button now reads "Continue".

## Bug Fixes

- **DCQL holder binding** is now enforced correctly. The value handed to the matcher was hardcoded to `true`, so the requirement was effectively never checked; it is now derived from the presentation itself, the spec default (`true`) is applied to stored queries, and a new `holder_binding` check is recorded. DCQL failures are reported as distinct, named error types per mismatch instead of one generic error.
- `mso_mdoc` credentials from a template with no expiry no longer get a hardcoded 1-year `validUntil`; a far-future sentinel is used. Credential Template previews and the Test Issue page no longer show a fabricated "today" validity and now reflect the template's actual saved validity. Issuer metadata `display` returns the template's real validity fields.
- `POST /system/certificates/csrs` builds the CSR subject from its fields (keeps `,`, `+`, `=` in values); `POST /system/certificates/import` rejects invalid `x5c` items with `400` instead of `500`; `DELETE` rejects managed certificates that have ever been active with `409`.
- Test Issue page surfaces the real validation error (instead of `[object Object]`) and seeds placeholder claims with type-correct defaults. Color fields accept 3-digit hex. `POST /:walletId/credentials/import` now whitelists `meta.kid` / `meta.did`. SelectID `client_uri` points to `/verify/information`.
- **Admin UI consistency:** consistent human-readable credential-format labels, "Date …" column headings, copyable long identifiers, a shared em dash for empty values, Title Case labels, a single page description per page, stable table column widths (with horizontal scroll on narrow viewports), routed detail drawers that survive refresh, a working request-history refresh control, the official empty-state icon and "Powered by Meeco" footer, trimmed schema/template names, and clearer demo Issue/Verify copy.

## Security

- Dependency upgrades to close known vulnerabilities. DM Sans is now self-hosted: the Content Security Policy no longer allows `fonts.googleapis.com` / `fonts.gstatic.com`, and server-rendered pages use a system sans-serif stack.
- Upgraded `@meeco/sd-jwt` (1.3.1) and `@meeco/sd-jwt-vc` (2.4.0): SD-JWT presentation verification now checks the Key Binding JWT's `typ`, `sd_hash` and `iat` in the library.
