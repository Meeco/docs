# SVX 3.1.0 Release Notes

**Software Release Date:** 21 November 2025

**Summary:**

This release introduces multiple enhancements for SVX Verify including session security protections, performance improvements for credential operations, and user experience improvements. It also adds support for X.509 certificate-based authentication for verifiable presentations, Redis state management, and PostgreSQL TLS/SSL support.

## Organisation Wallet

Added support for X.509 certificate-based client authentication in presentation requests as defined in [OpenID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html). Verifiers can now authenticate to wallet providers using X.509 certificates and mechanisms described in Client Identifier Prefixes `x509_san_dns` and `x509_hash`.

- Added `x509_hash` client id prefix support for presentation requests.
- Added optional `presentation.request.x509_client_id_prefix` configuration. Defaults to `x509_san_dns` if `include_x5c` is set to true.

### Added
- System endpoints
  - Changed error responses to return JSON instead of HTML for consistency with success responses
- (Gateway) Added new configuration parameter `cors_allowed_origins` to make CORS allowed origins configurable via `env.json`. Defaults to ["*"] if not provided for backward compatibility.

### Fixed
- `DELETE /presentations/requests/:requestId`
  - Fixed to remove the corresponding presentation request in Redis, ensuring the deleted presentation request is no longer accessible after deletion
- Fixed service crash when credential_issuer.oidc_clients was missing from configuration, preventing a runtime TypeError: jsonConfig.credential_issuer.oidc_clients is not iterable
- Fixed multiple redis connection that were created by using RedisStateManager. Now it is one, shared connection per application.

## SVX Verify

We enhanced security for SVX Verify by adding session protection through signed cookies and addressing multiple security vulnerabilities. The application now stores a signed cookie for the session ID in the client's browser, verifying this cookie to ensure sessions are accessed from the correct device and restricting access from other devices. To enable cookie signing, you need to add svx_verify.cookie_secret to the configuration. Additional security improvements include automatically adding the `Secure` attribute to all cookies when HTTPS is detected with proper trust proxy settings for load balancers, improved Content Security Policy by removing `unsafe-inline` for scripts and styles, adding `rel="noopener noreferrer"` to external links, and implementing payload validation for the POST `/identity/sessions` endpoint.

By allowing the bridge wallet to issue/verify without saving to vault, it reduces storing unnecessary PII and improves the performance. This optional configuration can be controlled via the `credential_issuer.save_issued_credential_to_vault` setting.

- Added new OPTIONAL configuration `credential_issuer.save_issued_credential_to_vault` to allow issuing credentials without saving into the SVX Vault.
- Added Session Cookies for SVX Verify to protect session.
  - 401 will be returned for attempts to access the session URL without the correct cookie
  - new configuration added `svx_verify.cookie_secret`. If configured, session cookies will be signed
- Added payload validation for POST `/identity/sessions`
- Added dynamic footer logo and text based on `svx_verify.operating_company_name` config for Verify flow.
- `returnTo` parameter removed from `POST /identity/language`. UI now reloads the current page to the new language on completion of the http request.
- Merged session_errors table into sessions table to simplify data stored for SVX Verify Sessions. Only latest error for the session will be stored.
- Fixed various security related issues:
  - Fixed `unsafe-inline` for `scriptSrc` and `styleSrc` for improved security
  - Fixed `target="_blank"` not having the `rel="noopener noreferrer"` option configured
- Fixed missing terms URL and privacy URL in footer
- Fixed cookie security vulnerability: Added Secure attribute to all cookies when app.base_url uses HTTPS
  - Cookies now automatically set `Secure` flag based on base URL protocol (HTTPS detection)
  - Affects: SVX Verify session cookies, i18next `ui_locales` cookie, OIDC provider cookies, and authorization server cookies
  - Cookies are properly cleared with matching secure flag to prevent cookie leakage
  - Global `trust proxy` setting added to ensure correct HTTPS detection behind load balancers

## SVX API

Presentation definitions can now be edited. We felt that the flexibility this provides greatly benefits testing and development. As a result, presentation requests linked to it, not necessarily used the most recent version of the definition.

- Added `PATCH /presentation_definitions/:id` endpoint to update presentation definitions
  - Update `name`, `purpose`, `input_descriptors` (PEX type only), or `dcql_query` (DCQL type only)
  - Following cannot be updated:
    - Archived definitions
    - `type` field
- Added date range filtering support for `GET /presentation_definitions` endpoint
  - `created_at_from` and `created_at_to` parameters for filtering by creation date
  - `updated_at_from` and `updated_at_to` parameters for filtering by update date
- Changed `presentation_definition.purpose` from require to optional. Affected endpoints: `POST /presentation_definitions` and `PATCH /presentation_definitions/:id`.
- Fixed type filter not being applied to `GET /presentation_definitions` endpoint for archived presentation definitions
- Fixed uncaught error during creation of presentation definition with invalid DCQL query
- Fixed presentation verification for DCQL Query with `credential_sets`
- Fixed `GET /presentation_definitions` endpoint, Swagger documentation for all query parameters including pagination

### Fixed

- Fixed pagination query parameters to be of type integer instead of number in Swagger documentation as Krakend gateway expects it to be an integer and not number.
