# SVX 4.3.0 Release Notes

**Software Release Date:** TBD

**Summary:**

This is a feature release for the SVX Platform. On the wallet service it adds a **credential catalog** for installing standard credentials (mDL, EU PID, Age Verification and more) in one step, **IETF Token Status List** support so credentials in every format can be revoked and suspended, with status lists cached by the verifier, read-only **Keys and Certificates** management to the Dashboard and API, an **ISO 18013-5 Reader CA** for issuing reader authentication certificates, a new **`device_request`** verification template type, and **search** across the main Dashboard tables. It also consolidates SVX Verify configuration into a single Settings page, enforces DCQL cryptographic holder binding correctly, and includes a broad round of Dashboard consistency work, bug fixes and security hardening. The API gateway, SVX Utilities and Meeco Portal gain supporting changes.

⚠️ **Breaking change for OIDC deployments:** admin authentication moves to `config.json`. Set it there before upgrading — see [Changed](#changed).

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

![Administration › Keys](/.gitbook/assets/Release_4.3.0_Keys.png)

![Administration › Certificates](/.gitbook/assets/Release_4.3.0_Certificates.png)

### ISO 18013-5 Reader CA

- New runtime setting `verifier.reader_ca.enabled` (requires `verifier.enabled`) and an optional ES256 managed key `ReaderCAKey`. The issuing CA certificate is signed outside SVX and imported via `POST /system/certificates/import` (strict validation of CA certificates; others rejected with `invalid_reader_ca_certificate`).
- New endpoints (enabled only when the setting is on):
  - `POST /verifier/reader/certify` — issues reader authentication certificates for a batch of CSRs, signed by `ReaderCAKey` (configurable leaf validity and batch size).
  - `GET /verifier/reader/issuing-ca.pem` / `.der` — publishes the issuing CA certificate for wallets to trust.
  - `GET /verifier/reader/crl` — publishes a signed (empty) CRL referenced by every reader certificate.
- Dashboard: a **Reader authentication** section under Settings › Verifier (enable, certificate lifetime, certificates per request) showing the active signing certificate. New metric `reader_ca_certifications_total{result}`.

![Reader authentication settings](/.gitbook/assets/Release_4.3.0_Reader_Authentication.png)

### `device_request` verification templates

- New verification template type storing `doc_requests`, the JSON equivalent of ISO 18013-5 DocRequests, with an IntentToRetain flag per data element. These templates cannot be used to create presentation requests. The Dashboard JSON editor offers a Device Request starter and an mDL sample with structure checks, and hides these templates from the Test Verify and SVX Verify pickers.

![Device Request starter in the verification template JSON editor](/.gitbook/assets/Release_4.3.0_Device_Request_Template.png)

### Dashboard search

- Search boxes on the View Credentials, Verification Requests, API Keys and Administrator Accounts pages. The matching list endpoints accept a `search` query param (max 50 chars) matching name or ID case-insensitively; verifier request search also matches the request ID.

![Search on the View Credentials page](/.gitbook/assets/Release_4.3.0_Credential_Search.png)

### Issuance

- Ad-hoc issuance (`POST /issuer/credentials`) accepts optional `valid_at` / `expires_at` (ISO 8601) to override the template's validity for that issuance, across `dc+sd-jwt`, `jwt_vc_json` and `mso_mdoc`. Responses now include the issuer credential `id` for each credential.
- `POST /issuer/offers` (and `POST /demo/issuer/offers`) accept an optional `credential_offer_endpoint` that overrides the configured endpoint for that offer only, with a matching field on the Test Issue page.

### DC API credential issuance

- SVX can now issue credentials over the browser's **W3C Digital Credentials API**. A new wallet register-credential-offer endpoint accepts optional `credential_issuer_metadata` and `authorization_server_metadata` payload parameters to support the DC API issuance flow, and the demo Issue UI offers a DC API option when the browser supports it.

### Other additions

- Admin display names can be edited via `PATCH /admin/accounts/{id}` and the Dashboard; passkeys show the display name instead of a UUID.
- Test Verify page: exclude `state` from the request, set a verifier Redirect URI, and set an optional Request Name.
- `POST /verifier/requests/{id}/responses` accepts an optional `options.expected_origin` used as the `origin:` audience and `mso_mdoc` session-transcript origin for DC API response modes.
- SVX Verify receipts can show an optional HTTPS `contact_url`.

## Changed

- **Admin authentication is now static config.** `system.admin.auth_method` / `system.admin.idp` move from runtime settings to `config.json` and are no longer editable in the Dashboard (`auth_method` defaults to `passkey`). A migration removes these keys from stored settings — **OIDC deployments must set them in `config.json` before upgrading** (and remove `admin` from Helm `runtimeBootstrap.systemDocument`).
- **Dashboard navigation redesign:** SVX Design System icons, reordered sections (Dashboard, Credentials, SVX Verify, Administration, Test, Devtools), "Admin Accounts" → "Accounts", and Keys/Certificates added under Administration.
- **SVX Verify configuration consolidated:** session policy and Bridge integration moved from Settings › Verifier onto a single SVX Verify **Settings** page (`/svx-verify/configuration` redirects to `/svx-verify/settings`); the setup checklist and an amber "incomplete" marker live there. An incomplete SVX Verify setup no longer blocks saving verifier settings; `POST /verify/sessions` returns `409 svx_verify_not_configured` with the missing fields until setup is complete.
- SVX Verify contact **phone and email are now optional** (receipt shows a localised placeholder).
- Credential schema names are now unique **per format** rather than globally.
- Admin display names are now **required** (`POST /admin/invitations`) and **unique** (migration renames/suffixes duplicates).
- Credential configuration is cached in-memory per instance instead of Redis, bounded by a new `issuer.credential_configuration_cache_ttl` setting (default 300s). Settings and credential-configuration change notifications are now raised by PostgreSQL triggers.
- Public SVX Verify and authorization pages now send a CSP `form-action` directive (configurable via `system.security.headers.content_security_policy.form_action`); known redirect targets are allowed automatically.
- OpenID4VP credential selection honours the DCQL `require_cryptographic_holder_binding` requirement. DC API response origin now comes from `options.expected_origin` (falling back to `system.app_host`) rather than request headers.
- Dashboard forms mark required fields with `*` and show allowed-character / max-length hints. The verification success screen button now reads "Continue".

## Bug Fixes

- **DCQL holder binding** is now enforced correctly. The value handed to the matcher was hardcoded to `true`, so the requirement was effectively never checked; it is now derived from the presentation itself, the spec default (`true`) is applied to stored queries, and a new `holder_binding` check is recorded. DCQL failures are reported as distinct, named error types per mismatch instead of one generic error.
- `mso_mdoc` credentials from a template with no expiry no longer get a hardcoded 1-year `validUntil`; a far-future sentinel is used. Credential Template previews and the Test Issue page no longer show a fabricated "today" validity and now reflect the template's actual saved validity. Issuer metadata `display` returns the template's real validity fields.
- `POST /system/certificates/csrs` builds the CSR subject from its fields (keeps `,`, `+`, `=` in values); `POST /system/certificates/import` rejects invalid `x5c` items with `400` instead of `500`; `DELETE` rejects managed certificates that have ever been active with `409`.
- Test Issue page surfaces the real validation error (instead of `[object Object]`) and seeds placeholder claims with type-correct defaults. Color fields accept 3-digit hex. `POST /:walletId/credentials/import` now whitelists `meta.kid` / `meta.did`. SelectID `client_uri` points to `/verify/information`.
- **Dashboard consistency:** consistent human-readable credential-format labels, "Date …" column headings, copyable long identifiers, a shared em dash for empty values, Title Case labels, a single page description per page, stable table column widths (with horizontal scroll on narrow viewports), routed detail drawers that survive refresh, a working request-history refresh control, the official empty-state icon and "Powered by Meeco" footer, trimmed schema/template names, and clearer demo Issue/Verify copy.
- **Meeco Portal:** creating an organization or tenant now redirects to the new entity's dashboard instead of the list page; viewing an Application no longer shows a spurious "Leave site?" prompt when the page is refreshed or closed.

## Platform and infrastructure

- **API gateway:** the gateway now proxies the new wallet endpoints — `GET /system/keys`, `GET /system/certificates/{id}`, `POST /system/certificates/{id}/activate` and `/deactivate`, `GET /issuer/status_lists` and `PATCH /admin/accounts/{id}`.
- **SVX Utilities:** now automatically uninstalls an organization's or tenant's wallets when it is archived or deleted, so wallets are cleaned up without manual intervention (enabled via `auto_deployments.process_atom_jobs`).
- **Meeco Portal:** upgraded to Node.js 24 and TypeScript 6 (with updated ESLint tooling).

## Security

- Dependency upgrades to close known vulnerabilities. DM Sans is now self-hosted: the Content Security Policy no longer allows `fonts.googleapis.com` / `fonts.gstatic.com`, and server-rendered pages use a system sans-serif stack.
- Upgraded `@meeco/sd-jwt` (1.3.1) and `@meeco/sd-jwt-vc` (2.4.0): SD-JWT presentation verification now checks the Key Binding JWT's `typ`, `sd_hash` and `iat` in the library.
- **Meeco Portal:** the application Client Secret is no longer stored in `localStorage`, and dependency vulnerabilities were reduced by upgrading 31 vulnerable packages.
