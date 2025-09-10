# SVX 3.0.0 Release Notes

**Software Release Date:** XX September 2025

**Summary:** 

SVX 3.0.0 is a major release that introduces SVX Verify, expands standards support, and delivers operational improvements across the platform. With this release, we continue our commitment to providing a future-proof foundation for high-assurance digital identity and credential exchange.

- **SVX Verify**: Launch of a new hosted service for low-friction, high-assurance identity verification, supporting both wallet-based and account-based models.

- **Self Signup**: New customers can now sign up and access the SVX Sandbox independently, accelerating onboarding and testing.

- **Resource Hook**: The Organisation Wallet can now fetch claims from external resource servers during credential issuance, simplifying integration with existing business systems.

- **OpenID4VCI & OpenID4VP v1.0 Support**: Full support for the final 1.0 versions of these standards, aligning SVX with the broader ecosystem.

- **DCQL Support**: Introduction of the Digital Credentials Query Language, providing a simpler, real-world-focused alternative to DIF’s Presentation Exchange.

- **DPoP Support**: Optional Demonstrating Proof of Possession support added to Organisation and Holder Wallets for stronger token security.

- **Updated IETF SD-JWT VC (draft-08)**: All components updated to support the latest draft, including migration from vc+sd-jwt to dc+sd-jwt.

- **Unified Configuration Framework**: All components now use JSON-based configuration backed by JSON Schema, ensuring consistency, validation, and easier operations.

This release also includes multiple usability enhancements, bug fixes, and security updates across the Portal, SVX API, Organisation Wallet, and Holder Wallet.

Here are the highlights of the new features introduced in SVX 3.0.0:

# New Features
## SVX Verify
Identity verification is a critical step in digital journeys that require organisations to know the users' identity. Traditional approaches rely on document scanning and can be slow, costly and error prone. They introduce friction for users and complexity for organisations, all while creating new risks around oversharing of PII and compliance issues that come along with that.

SVX Verify offers a new way forward. Instead of relying on document scans, it leverages high-assurance digital identity solutions to deliver:

- **Streamlined identity verification**: A user flow causing fewer drop-offs, higher conversions and overall reduced friction.

- **Go Live in Minutes**: Use our hosted user experience to start verifying credentials with minimal setup or integration effort.

- **Future proof**: Support for standards-based digital credentials as they roll out.

### How it works
SVX verify makes it simple for organisations to integrate high-assurance identity verification into their workflows:

1. **Define** the identity attributes you’re looking to verify

2. **Redirect to SVX Verify**: From any integrated system, users are redirected to the hosted SVX Verify flow

3. **Identity verification**: Users verify their identity either by

    - Wallet-based credentials: Presenting digital credentials such as ISO Mobile Documents and IETF SD-JWT VCs  directly from a wallet.

    - Account-based providers: Authorising a trusted identity provider to share verified attributes using OpenID Connect

4. **Consent and security built-in**: SVX Verify managed consent, data sharing and error handling, ensuring privacy and compliance.

5. **Retrieve verified data**: Verified attributes are retrieved via an API or made available in Portal.

Below is a journey that shows how SVX Verify can be used in hotels to enable online check-in using a wallet-based system:

<p align="center">
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_SVXVerify_Wallet01.png" alt="SVX Wallet flow 01" width="80%">
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_SVXVerify_Wallet02.png" alt="SVX Wallet flow 02" width="80%">
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_SVXVerify_Wallet03.png" alt="SVX Wallet flow 03" width="80%">
</p>

### Bridging today and tomorrow
SVX Verify is designed to fully embrace the new generation of secure, privacy-preserving digital credentials. As governments worldwide adopt standards such as ISO Mobile Documents and IETF SD-JWT VCs, SVX Verify provides organisations with a pathway to accept these high-assurance credentials as they roll out.

At the same time, adoption will vary by country and ecosystem. Many markets already offer digital identity services through APIs and regulated identity providers. SVX Verify includes a bridge component that connects to these account-based ecosystems, allowing organisations to accept trusted identities today while creating derived credentials for use across their own networks.

### Built on Organisation Wallet
SVX Verify is built on the SVX Organisation Wallet. It provides a hosted UI for end-users and an API-first model for organisations, ensuring rapid deployment without complex setup. By bridging today’s account-based identity providers with tomorrow’s wallet-based credentials, SVX Verify gives organisations a future-proof way to verify identity with high assurance, low friction, and full compliance.

## Self Signup to SVX
Previously, new users had to contact Meeco directly to access the SVX Sandbox environment. We have now introduced a self signup form, allowing users to register themselves and begin testing SVX functionalities more quickly and easily.

This feature can be enabled/disabled on a per installation basis.

<img style="display: block; margin: 0 auto;" src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_SelfSignup.png" alt="Self Signup to SVX link image" width="50%">

## Issuance via Resource Hook
The Resource Hook is a new feature in the SVX Organisation Wallet that enables credential issuance using data from an external resource server.

When a credential issuance request is received, the Organisation Wallet automatically calls the configured resource server to fetch the required data. The returned values are then included as claims in the issued credential.

This approach separates responsibilities:

Organisation Wallet manages the technical aspects of issuance flows and ensures compliance with open standards (e.g. OpenID4VCI).

External resource server handles the business logic and provides the data that is included as claims in the credential.

This allows organisations to integrate the Organisation Wallet seamlessly into their existing systems without duplicating or moving business logic into the wallet itself.

The Resource Hook functionality can be configured directly in the Organisation Wallet.

For more details on configuration options, refer to Appendix B.

# Standards Updates
## OpenID4VCI and OpenID4VP v1.0 Support
After numerous years of being under development, both OpenID specifications have recently published a final version (1.0). We’re extremely proud to announce that the SVX Platform fully support Issuers, Verifiers and Wallet Providers in using these protocols in conjunction with ISO Mobile Documents, IETF SD-JWT VC and W3C Verifiable Credentials.

Below are some notable new features that are supported:

### Support for DCQL
DCQL is defined in section 6 of the [OpenID for Verifiable Presentations v1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-digital-credentials-query-l) specification:

> The Digital Credentials Query Language (DCQL, pronounced [ˈdakl̩]) is a JSON-encoded query language that allows the Verifier to request Presentations that match the query. 

DCQL simplifies the complexity of Presentation Exchange (PEX) by focusing on real-world use cases, offering a simpler design, practical features, and a specification tailored to the OpenID4VP protocol. For more information about DCQL, refer to [OpenID for Verifiable Presentations 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-digital-credentials-query-l). Refer to [Appendix A](#Appendix A - DCQL Query Examples) for examples of DCQL Query.

Note that DIF Presentation Exchange (PEX) implementation remains unchanged and can still be used.

### Support for DPoP
As recommended in the OpenID4VCI specification, under [Best Current Practice for OAuth 2.0 Security](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-best-current-practice-for-o), support for DPoP has been added to both the Organisation Wallet and the Holder Wallet. 
DPoP support is optional and can be enabled via component configuration.

Demonstrating Proof of Possession (DPoP) allows securing an access token by binding a cryptographic key into it. Usage of the access token is only allowed when a valid proof JWT is sent alongside in the request header. For more information about DPoP, refer to [RFC 9449: OAuth 2.0 Demonstrating Proof of Possession (DPoP)](https://datatracker.ietf.org/doc/html/rfc9449).

This complements really well the OAuth 2.0 Proof Key for Code Exchange (PKCE), which secures the exchange of the authorization code received in the authorization code flow. Both are mandatory in the [FAPI 2 Security Profile](https://openid.net/specs/fapi-security-profile-2_0.html), often considered an industry standard when it comes to security.

### Transition to Final
The publication of the final 1.0 specifications is a major milestone and a clear signal to the digital credential ecosystem. While future iterations will emerge, Meeco is committed to treating this release as a **long-term supported (LTS) baseline**, ensuring stability and backwards compatibility for our customers.

During the draft process we introduced parameters such as `protocol_version` to handle multiple draft versions (e.g. OpenID4VP draft 10 and draft 18). However, maintaining compatibility with drafts creates unnecessary fragmentation. Each draft differs slightly, and no other ecosystem players are continuing to support them.

To promote alignment and reduce integration complexity, this release **drops all support for draft versions and only accepts the final identifiers**:

- `openid4vci-v1`
- `openid4vp-v1`

These values are also applied by default when the parameter is not specified.

By consolidating on the final 1.0 specifications, we simplify integration, remove ambiguity, and ensure that our customers are fully aligned with the broader ecosystem moving forward.

## Updated IETF SD-JWT VC to draft 08
As part as our commitment to follow and support most used credential formats in production use cases, we’ve updated all components to support draft 08 of the [IETF SD-JWT-based Verifiable Credentials](https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/08/) specification.

The most notable change introduced in this draft is a new credential format identifier:

- **From** `vc+sd-jwt` 
- **To** `dc+sd-jwt` 

# Operating Updates
## Unified (JSON-based) Configuration Framework
To make day-to-day operations easier, we’ve standardised how configuration for components is defined, validated and documented across the SVX Platform.

Previously, components used different formats (YAML, dotenv, etc.). All SVX components now use a single configuration format (JSON) with consistent file names and key structures across the platform.  Common settings, such as database connections, are configured in the same way, no matter the programming language and framework used.

Every config is backed by a JSON Schema, which is validated automatically at container startup. This ensures that outdated or invalid configs fail fast, making misconfigurations easy (or at least easier) to detect and fix.

The JSON Schema also serves as living documentation, providing descriptions for every config key.

# Component Updates
## Portal
### DCQL Support
Portal has been updated to support DCQL query creation and processing.

- Added support for creation of a `Verification Template` with DCQL Query. `Create Verification Template` page has now been separated into two tabs, Visual Builder (PEX) and JSON Editor (DCQL)
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_Portal_DCQLSupport.png" alt="JSON Editor (DCQL) in the Create Verification Template page" width="80%">

- Added support to view DCQL Query verification responses
- Added support for viewing `Verification Templates` with DCQL Query format

### New functionalities
- Added detailed view of `constraints` `format` and `signature algorithm` for `View Presentation Template` page.
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_Portal_New_VerificationTemplates.png" alt="Detailed view of constraints, format and signature algorithm in the View Presentation Template page" width="80%">

- Added support for credentials using `dc+sd-jwt` format identifier.
- Added a link from the `Tenant/Organization Details` page to the corresponding Dashboard.
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_Portal_New_TenantDetails.png" alt="Link to Tenant in the tenant details page" width="80%">
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_Portal_New_OrganisastionDetails.png" alt="Link to Organisation in the organisation details page" width="80%">

- Added ability to Create and View the `Credential Type` `type` for W3C VC formats

### Enhancements
- Added `audience`, `IDP Login URL`, `Tenant ID` and `Organization ID` (if available) to applications. This makes it easier to configure system-to-system authentication using OAuth 2’s client credentials flow.

- Added confirmation code check when removing an end user from a tenant.
<img src="../.gitbook/assets/releases/3.0.0/Release_3.0.0_Portal_Changed_Enduser.png" alt="Added confirmation code check when removing an end user from a tenant" width="80%">

- Allowed Global Admins to view archived tenant details from the Tenants list.
- Updated the `jwt_vc_json` verification template example for presentation exchange (PEX) to align with OpenID4VP v1.0.
- `Organization Url` is no longer a required field when creating or editing an Organization, added regex validation.
- Changed `Disclosure Frame` editor from a plain text area, to a JSON code editor component for `Credential Templates` creation and editing.
- Changed `Disclosure Frame` to be editable for `Credential Templates`.

### Removed
- Removed support for creating credentials types and presentation templates using `vc+sd-jwt` format identifier. Replaced with `dc+sd-jwt` to align with OpenID4VI V1.0
- Removed the ability to add end-users to a tenant from the portal.
- Removed the `Pending End Users` tab from the End Users page, along with all related pages and functionalities.

### Bug Fixes
- Fixed parsing of non-array `vp_tokens` to show older verification_results correctly.
- Fixed an issue where the copied icon was not displaying correctly for copied values. 
- Removed duplicated ID field for global admin details in the View Administrator page.
- Removed the Edit button for archived schemas to prevent unintended modifications.
- Trimmed search input to remove unnecessary whitespace for `Tenants`, `Organisations`, `Credential Schemas`, `Credential Templates`, `Verification Templates`, and `Verification Responses`.
- Fixed missing "No results found for '{search string}'" message when searching archived Credential Templates, Verification Templates, and Verification Requests with no matches.
- Fixed missing "No archived Credential Templates / Verification Templates / Verification Requests found" message when no archived items are available for these sections.
- Fixed error handling for creating credential schema - the page no longer hangs when the schema name is too long and an appropriate error message is shown.
- Updated the following error messages to be more user-friendly: `field_too_long`, `invalid_company_url`, `agent_with_this_name_already_exists`.
- Fixed validation to correctly detect syntax errors in uploaded JSON files on the `Create Schema` page.
- Fixed an issue where error messages persisted during tenant creation and when adding an administrator.
- Fixed the Add button on the `Add Administrator` page to only become clickable after input fields are valid.
- Fixed the Create button in the `Create Credential Template` and `Create Verification Template` page to only become clickable after input fields are valid.
- Fixed areas that were missing Japanese translations.
- Fixed `Format` on `Credential Types` tables displayed the format for mdoc as its doctype
- Fixed `valid at issuance` so that the `valid_at` value is set to null for credential types, ensuring the credential is valid at the time of issuance.
- Fixed credential type preview `Valid From` to calculate based on the current timestamp instead of the credential type’s creation date.
- Fixed a bug that didn't accept the edited version of the `Disclosure Frame` when creating a `Credential Templates`.
- Fixed an issue where the docType `org.iso.18013.5.1.mDL` was automatically converted to `Mobile Driving License (mDL ISO/IEC 18013-5:2021)`. All credentials using Mobile Document show as (ISO Mobile Document).
- Fixed the portal login page title to reflect the correct title. It is also configurable in IDP with the config setting `branding` -> `app_title`.

## SVX API

### OpenID4VP v1.0 Support
- Added new parameters to `POST /presentation_definitions` to add support for **DCQL Query**  
  - `type` - `'pex' | 'dcql'`
- Added new properties to `POST /presentation_request` response  
  - `parameters.dcql_query`  
  - `parameters.presentation_definition_type`
- Added new `model_version` value `dcql-openid4vp-v1` to `PresentationDefinition`
- Added `type` query parameter to `GET /presentation_definitions` endpoint

### Self-Signup

#### New Endpoints
- `GET /signup` - Display public signup registration form
- `POST /signup` - Submit signup registration with reCAPTCHA validation
- `GET /signup/success` - Show signup confirmation page
- `GET /signup/resend-verification` - Display resend verification email form
- `POST /signup/resend-verification` - Resend verification email with reCAPTCHA validation
- `GET /signup/resend-verification/success` - Show resend verification confirmation page

#### Features
- Email verification workflow with token-based validation
- reCAPTCHA protection on all form submissions
- Feature toggle support via `signup.enabled` config variable
- Integration with existing notification system for verification emails
- Automatic user and tenant creation upon successful email verification
- Company name uniqueness validation to prevent duplicate tenant names

#### Configuration Properties
- `enabled` - Whether user signup is enabled
- `verification_token_expiry_seconds` - Signup verification token expiry time in seconds
- `resend_verification_cooldown_seconds` - Signup cooldown period for resending verification emails
- `confirmation_email_recipient` - Email address to receive signup confirmations

### New Functionalities
- Added an endpoint `POST /tenant/with_admin/{user_id}` that allows creating a new tenant where the first admin is not the current user but the user specified by the `user_id` parameter.
- Enhanced invitation creation with email normalization (case-insensitive).
- Added a public endpoint `GET /orgs/{org_id}/logo` that redirects a client to the logo of the organisation.

### Enhancements
- Latest **SD-JWT-VC draft-08** related changes.
- Updated the credential type format identifier from `vc+sd-jwt` to `dc+sd-jwt` for SD-JWT credential types.
- The credential header `typ` for SD-JWT is now set to `dc+sd-jwt` for new credential generation.  
  - Verification continues to support both `sd-jwt-vc` and `dc+sd-jwt` format identifiers to ensure backward compatibility.
- `POST /openid/presentations/requests` payload parameter `scope` no longer defaults to `openid`.  
  - If nothing provided, it will be `null`.
- Updated password minimum length requirement to **12 characters**.

### Removed
- Removed support for **OpenID4VP draft-18**  
  - `openid4vp-draft18` presentation requests created in the past are still returned by the API, but no new ones can be created.  
  - Use `protocol_version = openid4vp-v1` instead.
- Removed `client_id_scheme` payload parameter from `POST /openid/presentations/requests`.

### Bug Fixes
- Fixed storing events in development when `users.login_id` (username) is not a UUID.
- Fixed storing `ItemDeleted` events correctly.

### Security Updates
- Upgraded **Node** to `22.14 LTS`
- Upgraded **Ruby** to `3.4.5`
- Upgraded **Erlang** to `27.3.4.2`
- Updated **Elixir** to `1.18.4`

## Organisation Wallet

### SVX Verify

#### Pages
- `/identity/verification/:id` `SessionStep = Index`  
  Verifier landing page.
- `/identity/verification/:id` `SessionStep = Share`  
  Displays QR code for wallet-based flows or a "Sharing Credentials..." screen for account-based flows.
- `/identity/verification/:id` `SessionStep = Complete`  
  Displays the results of the verification process (success).
- `/identity/verification/:id` `SessionStep = Error`  
  Displays error results with `error_code`, `error_description`, and `error_tips`.

#### Endpoints
- Added verify layout for all `/identity/verification/` routes.
- `GET /identity/verification/:id/status` – Check and return status of verification flow session.
- `GET /identity/verification/:id/request` – Return identity verification presentation request token.
- `POST /identity/verification/:id/idp/:idp` – Update session with selected IdP.
- `POST /identity/verification/:id/abort` – Abort session, returning to `returnURL`.
- `POST /identity/sessions` – Session management; accepts `return_url`, `success_url`, and `presentation_template_id`.
- `GET /identity/sessions/:id` – Fetch session state and `verified_claims`.
- `POST /identity/language` – Language switching; accepts `ko`, `jp`, `en`, `zh-hk`.
- `GET /identity/information` – Render `client_uri` for Select ID integration.

#### Configuration Changes
- `svx_verify.enabled` – Enable/disable `/identity/` routes.
- `svx_verify.brand_name` – Configure brand name.
- `svx_verify.operating_company_name` – Configure operating company name.
- `svx_verify.presentation_template_ids` – Configure available Presentation Templates.
- `svx_verify.response_mode` – Control IDP verification response mode.
- `svx_verify.session_expiry` – Session expiry time.
- `svx_verify.active_idps` – Available IDPs.
- `svx_verify.bridge_authorize_url` – Bridge authorize entrypoint.
- `svx_verify.bridge_client_id` – Bridge authorization code flow.
- `svx_verify.presentation_request_multiple_loads_enabled` – Allow multiple request loads.
- `svx_verify.manual_terms_acceptance_enabled` – Require "Accept" checkbox.
- `svx_verify.verifier_information` – Configure organisation verifier information.

### OpenID4VCI v1.0 Support
#### Configuration Changes
- Added `protocols.issuance = openid4vci-v1`.
- Added `credential_issuer.max_batch_credential_issuance_size`.
- Added `credential_issuer.oidc_clients` to register Issuer IDP clients.
- Added optional `credential_issuer.include_x5c`.
- Renamed `POST /credentials` → `POST /credential` (spec update).
- Added `background_image` to credential metadata.
- Updated credential metadata in `/.well-known/openid-credential-issuer` to match spec.  
  - `mso_mdoc` credential returned as base64url-encoded `issuerSigned` object.

### DPoP Support
- `POST /token` and `POST /credential` endpoints.  
- Added `credential_issuer.dpop_enabled` configuration.

### OpenID4VP v1.0 Support
- Added `openid4vp-v1` to `protocols.presentation`.
- New `response_mode` options: `dc_api`, `dc_api.jwt`.
- Supported client_id prefixes: `redirect_uri`, `decentralized_identifier`, `x509_san_dns`.
- Added `expected_origins` parameter to `POST /presentations/requests`.
- `presentation request` now contains `client_metadata.vp_formats_supported`.
- `authorization_encrypted_response_alg` → replaced by `alg` from JWKS.
- `authorization_encrypted_response_enc` → renamed to `encrypted_response_enc_values_supported` (array).
- `scope` no longer defaults to `openid` — must be explicit.
- `submissionId` → renamed to `submission_id` in `POST /openid/presentations/requests/:requestId/submissions`.

#### Configuration Changes
- Added optional `presentation.request.include_x5c`.
- Added required `presentation.request.default_response_mode`.

### Bridging Entity
- New config: `credential_claims_map` (map external source claims).
- Added claim display options: `essential_claims`, `optional_claims`.
- Added optional config groups under `integrations`.
- Added `POST /interaction/:uid/select_authorization_server` – Generic bridge form handler.
- Added `select_authorization_server` render function for bridge connections.

### Resource Hook
- Feature: fetch claims from resource server.
- New config group `resource_hook`:  
  - `endpoint` – Resource server URL.  
  - `send_schema_json` – Include schema JSON (debugging).

### IETF SD-JWT VC (draft-08) Support
- Updated credential type format identifier: `vc+sd-jwt` → `dc+sd-jwt`.
- Credential header `typ` now `dc+sd-jwt`.

### New Functionalities
- Added `?ui_locales=en` query param + cookie for language selection.
- Added translations: English, Japanese, Chinese (HK), Korean.
- Added `verifierRedirectUri` param to `POST /presentations/requests`.
- Added `redirect_uri` in response of `POST /openid/presentations/requests/:requestId/submissions` when `verifierRedirectUri` is set.
- Added config `presentation.request.default_expected_origins`.
- **DCQL Support** (OpenID4VP v1):  
  - Create request with `dcql_query`.  
  - Verify response with `dcql_query`.
- Added `VerifierError` for unexpected front-end route errors.
- Added PostgreSQL support (`postgres` config).
- Added persistent identity verification session model.

### Enhancements

#### Application Config
- Moved `c_nonce_expires_in` → `credential_issuer.c_nonce_expires_in`.
- Moved `oid4vc.redis_state_manager_key_expiration_time` → `state_manager.default_expiry`.
- Removed `oid4vc` key.
- Renamed `svx.organization.auth_uri` → `svx.organisation.token_uri`.

#### Test UI
- String fields render as textarea (or date input if format="date").
- Added `dark:text-white` for field labels + white background for QR code.
- Boolean fields → rendered as radio buttons.
- Integers accepted as-is (not coerced to strings).

#### Certificate Checks
- Require `O`, `C`, `CN` attributes.  
- `CN` must match `app.base_url`.  
- `subjectAltName` must match `DNS:app.<base_url>`.

#### Credential Type Validation
- Duplicate `vct` or `doctype` removed from `supportedCredentialTypes`.  
- Errors logged at startup.  
- `GET system/svx/reload_data` returns **400** if duplicates exist.

#### Endpoint Changes
- `POST /presentations/request` response → snake_case keys.
- `POST /presentations/requests/:id/stats` response → snake_case keys.
- `/interaction/:uid` now renders error page (not raw JSON).
- `getCredentialDisplayMetadata` uses credential type name (not schema name).
- Added support for `contentEncoding` and `contentMediaType` in JSON schema.  
  - Encodings: `base64`.  
  - Media types: `image/png`, `image/jpeg`, `image/svg+xml`.

### Removed
- Dropped support for **OpenID4VCI-draft13**.
- Removed `openid4vci-draft13` from `protocols.issuance`.
- Removed non-standard claims: `sub`, `client_name` from requests.
- Removed `submission_id` from `POST /openid/presentations/requests/{requestId}/submissions`.
- Removed `supported_credential.<ID>.expiry` → replaced by `expires_at`, `expires_in`.
- Removed support for **OpenID4VP draft-18**.
- Removed `credential_issuer.supported_credential` properties → replaced by SVX API:  
  - `vct`, `expires_at`, `expires_in`, `valid_at`, `valid_ion`, `disclosure_frame`.
- Removed `sd_jwt_vc.reserved_claim_keys` → now defaults to library list.

### Bug Fixes
- Added validation for `POST /openid/presentations/requests/:requestId/submissions` – prevent empty `vp_token` (`'[]'` string).
- Fixed crash when `credential_types` blank in `/test/issue`. Added backend validation + enforced frontend requirement.
- Fixed `x5chain` in `COSE_Sign1` for `mso_mdoc` (single cert as byte array string).
- Fixed bad JSON payload error handling – return **400** instead of **500**.
- Fixed security declaration of public endpoints in `api-spec.yaml`.

### Security Updates
- Upgraded **oidc-provider** to `8.8.1`

## Holder Wallet
### Bridging Entity
#### Configuration
- Added `bridge_wallet` object:
  - `enabled` – Turns on bridge wallet setup.
  - `external_reference` – External reference string for wallet initialisation.
  - `wallet_key_type` – Defines the `kty` and `crv`.
  - `issuer_wallets` – Contains information related to IDPs used for Bridge issuance flow.

#### Endpoints
- Added `/authorize` endpoint to initialise the `bridge_wallet` flow.

### New Functionalities
- **DPoP support** – Detects `dpop_signing_alg_values_supported` from `/.well-known/openid-configuration`.
- Added `SetupService` provider to `AppModule` for post-initialization setup.
- Supports **PEX** and **DCQL** Presentation Requests.
- Added `/authorize/receive/callback` endpoint for `bridge_wallet` initiated flows to return to.
- Added credential format type `dc+sd-jwt` for SD-JWT credential types.
- Added **OpenID4VP DCQL Support**:
  - Register Presentation Request with `dcql_query`.
  - Submit Response with `dcql_query`.
- Added **OpenID4VP v1.0 Support**:
  - `POST /wallets/:id/send` now supports `protocol_version = openid4vp-v1`.
  - Added validation for `x509_san_dns` Verifier Client Identifier Scheme to ensure:
    - Identifier is a URL.
    - Matches `dNSName` SAN entry of x.509 leaf certificate.
- Added **OpenID4VCI v1.0 Support**:
  - New default and only issuance `protocol_version = openid4vci-v1`.
  - Added optional `key.client_assertion_jwk` to enable `private_key_jwt` client auth.

### Enhancements
- Updated to **SD-JWT-VC draft-08** changes:
  - Mandatory JOSE Header `typ` set to `dc+sd-jwt`.
  - Optional metadata (`type` as URL) deferred to future.
- Legacy `vc+sd-jwt` format is now **deprecated**.
  - New issuance & presentation requests → use `dc+sd-jwt`.
  - Existing `vc+sd-jwt` credentials continue to work for storage, verification, and presentation.
- Changed `GET /wallets/{:id}/receive/{:state}`:
  - `proof` → replaced with `proofs`.
  - `proofs.jwt` always returned as an array.
- `mso_mdoc` issuance now expects **base64url encoded `IssuerSigned`** (per OpenID4VCI v1.0 spec).  
  Previously expected a `DeviceResponse`.

### Removed
- From `GET /wallets/{:id}/receive/{:state}` response:
  - `c_nonce`
  - `c_nonce_expires_in`
- From `POST /wallets/{:id}/receive/get_access_token` response:
  - `c_nonce`
  - `c_nonce_expires_in`
- From `POST /wallets/{:id}/receive/get_credential` response:
  - `c_nonce`
  - `c_nonce_expires_in`
- Removed support for:
  - **OpenID4VP draft-18**
  - `client_id_scheme` (no longer separate from `client_id`)
  - `client_metadata_uri`
  - `pre_registered_client_id_scheme_default_metadata` configuration.

### Bug Fixes
- Fixed incorrect SVX host config variable reference in **DID Service Factory**.
- Fixed issue when creating `mso_mdoc` device response in **mixed-format** presentation definitions (e.g., with `jwt_vc_json` or `dc+sd-jwt`).  
  Previously, it incorrectly matched `mso_mdoc` VCs against input descriptors of other formats.
- Fixed security declaration of public endpoints in **OpenAPI spec**.

### Deprecations & End of Life
- Credential format identifier `vc+sd-jwt` is **deprecated**.  
  Use `dc+sd-jwt`.
- Removed protocol version support:
  - **OID4VP**: `openid4vp-draft18` → use `openid4vp-v1`.
  - **OID4VCI**: `openid4vci-draft13` → use `openid4vci-v1`.

# Appendix
## Appendix A - DCQL Query Examples
### `jwt_vc_json`
> When used in the context of W3C Verifiable Credentials, the claims_path parameter always matches on the root of Verifiable Credential (not the Verifiable Presentation).

Example `jwt_vc_json` credential
````
{
  "iss": "https://example.gov/issuers/565049",
  "nbf": 1262304000,
  "jti": "http://example.gov/credentials/3732",
  "sub": "did:example:ebfeb1f712ebc6f1c276e12ec21",
  "vc": {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://www.w3.org/2018/credentials/examples/v1"
    ],
    "type": [
      "VerifiableCredential",
      "IDCredential"
    ],
    "credentialSubject": {
      "given_name": "Max",
      "family_name": "Mustermann",
      "birthdate": "1998-01-11",
      "address": {
        "street_address": "Sandanger 25",
        "locality": "Musterstadt",
        "postal_code": "123456",
        "country": "DE"
      }
    }
  }
}
````

#### Simple query
````
{
  "credentials": [
    {
      "id": "example_jwt_vc",
      "format": "jwt_vc_json",
      "meta": {
        "type_values": [["IDCredential"]]
      },
      "claims": [
        {"path": ["credentialSubject", "family_name"]},
        {"path": ["credentialSubject", "given_name"]}
      ]
    }
  ]
}
````

### `dc+sd-jwt`
#### Simple query
````
{
  "credentials": [
    {
      "id": "pid",
      "format": "dc+sd-jwt",
      "meta": {
        "vct_values": ["https://credentials.example.com/identity_credential"]
      },
      "claims": [
        {"path": ["given_name"]},
        {"path": ["family_name"]},
        {"path": ["address", "street_address"]}
      ]
    }
  ]
}
````

### `mso_mdoc`
#### Simple query
````
{
  "credentials": [
    {
      "id": "my_credential",
      "format": "mso_mdoc",
      "meta": {
        "doctype_value": "org.iso.7367.1.mVRC"
      },
      "claims": [
        {"path": ["org.iso.7367.1", "vehicle_holder"]},
        {"path": ["org.iso.18013.5.1", "first_name"]}
      ]
    }
  ]
}
````

## Appendix B - Resource Hook
Resource Hook is a feature to call an external resource server when a credential is requested via OpenID4VCI protocol.

Non-normative example of resource hook configuration:

````
"resource_hook": {
  "endpoint": "http://resource-server.com/get-claims",
  "send_json_schema": false
}
````
A http request to the configured `resource_hook.endpoint` will be sent with the following payload
````
"user_id": "some_user_id", // user_id in access_token
"format": "dc+sd-jwt", // format identifier of requested credential
"timestamp": "1757421906693", // number of milliseconds since Unix epoch (1970/01/01)
"vct": "vct", // dc+sd-jwt specific credential identifier
"doctype": "doctype", // mso_mdoc specific credential identifier
"type": ["credential-type"], // jwt_vc_json specific credential identifier
"schema": { /** Credential Schema associated with the Credential Template **/ }
````
The format specific credential identifier(s) will only have value when it is the corresponding format identifier.

For example, if a request for a sd-jwt vc is received by the SVX Organisation Wallet, the following will be sent to the resource server.
````
{
  "user_id": "some_user_id",
  "format": "dc+sd-jwt",
  "timestamp": "2025-09-09T05:40:29Z",
  "vct": "studentID"
}
````
`schema` is only sent if `resource_hook.send_json_schema` is set to `true`.
It is meant to help with testing and development.
It is expected that the resource server would be able to identify the required claims based on the format specific credential identifier.
Sending the `schema` for each request is additional payload that should be not necessary.

For security considerations, `X-SVX-OCW-Signature` will be sent along in the HTTP header.
It is a signature of the JSON string of the payload, signed with the same key that signs credentials. (SVX Organisation DID or the key of the configured `credential_issuer.signing_kid`)
This can be used to verify that the call is from the expected SVX organisation wallet component.

The expected response is of the following format:
````
// object containing claims
"claims": {
  "foo": "bar",
}, 
````