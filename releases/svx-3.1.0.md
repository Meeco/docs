# SVX 3.1.0 Release Notes

**Software Release Date:** 21 November 2025

**Summary:**

This release introduces multiple enhancements for SVX Verify including session security protections, performance improvements for credential operations, and user experience improvements. It also adds support for X.509 certificate-based authentication for verifiable presentations, Redis state management, and PostgreSQL TLS/SSL support.

# Highlights
## Enhancements for SVX Verify
### Session Protection
We added an extra layer of security to SVX Verify sessions by storing a signed cookie for the session ID in the client's browser. The application verifies this cookie to ensure the session is accessed from the correct device and restricts other devices from accessing it. To enable cookie signing, you need to add svx_verify.cookie_secret to the configuration.

### Optional Vault Storage for Credentials
By allowing the bridge wallet to issue/verify without saving to vault, it reduces the operational steps which improves the performance. This can be controlled using the optional configuration `credential_issuer.save_issued_credential_to_vault`.

### Security Enhancements
We made security enhancements to SVX Verify, including fixing cookie security vulnerabilities by automatically adding the `Secure` attribute to all cookies when HTTPS is detected, with proper trust proxy settings for load balancers. We also improved Content Security Policy by removing `unsafe-inline` for scripts and styles, added `rel="noopener noreferrer"` to external links, and implemented payload validation for the POST `/identity/sessions endpoint`.

## X.509 Certificate-Based Client Authentication for Verifiable Presentations
Added support for X.509 certificate-based client authentication in presentation requests. Verifiers can now authenticate using X.509 certificates, with the client_id derived from the certificate's Subject Alternative Name (DNS) or certificate hash.

# Component Updates
## Organisation Wallet
### SVX Verify
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

### X.509 certificate-based client authentication for VP
- Added `x509_hash` client id prefix support for presentation requests.
- Added optional `presentation.request.x509_client_id_prefix` configuration. Defaults to `x509_san_dns` if `include_x5c` is set to true.

### Added
- System endpoints
  - Changed error responses to return JSON instead of HTML for consistency with success responses

### Fixed
- `DELETE /presentations/requests/:requestId`
  - Fixed to remove the corresponding presentation request in Redis, ensuring the deleted presentation request is no longer accessible after deletion
- Fixed service crash when credential_issuer.oidc_clients was missing from configuration, preventing a runtime TypeError: jsonConfig.credential_issuer.oidc_clients is not iterable
- Fixed multiple redis connection that were created by using RedisStateManager. Now it is one, shared connection per application.

## Organisation Wallet Gateway
### Added
- Added new configuration parameter `cors_allowed_origins` to make CORS allowed origins configurable via `env.json`. Defaults to ["*"] if not provided for backward compatibility.