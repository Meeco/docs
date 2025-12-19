# SVX 3.2.0 Release Notes

**Software Release Date:** 26 November 2025

**Summary:**

This release delivers four major feature areas across the SVX platform, Organisation Wallet and Portal. The key outcomes are improved autonomy for organisations, faster template creation and management, more flexible credential workflows, and a significantly enhanced developer testing experience.

# New Features

## Self-Service Organisation Wallet Deployment

This release introduces self-service deployment of Organisation Wallets, giving administrators greater autonomy when testing and running proof-of-concept flows for issuing and presenting credentials. Organisations can now deploy their own wallets directly from the SVX portal with minimal support from Meeco.

### Key capabilities

- Deploy an Organisation Wallet directly from the application view within an organisation.
- Deployment status is shown in the UI and marked as "Online" once ready.
- Delete an existing deployment if it is no longer required.

As we are rolling out this feature, note that only one active Organisation Wallet deployment is allowed per organisation.

### How it works

1. Create a new application within your organisation.
2. Open the application details page.
3. Click the **“Deploy Organisation Wallet”** button.
<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_SelfService_OW_Deployment01.png" alt="Deploy Organisation Wallet button in the View application page">

4. Wait for the deployment to complete. The deployment is ready when the status shows **Online**.
<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_SelfService_OW_Deployment02.png" alt="Deployment status changed to online in the View application page">

5. Use the **Audience** URL displayed in the application to access and run your Organisation Wallet.
6. Once you set up credential/verification templates, you can use Organisation Wallet test UI to test out issuance and verification.
    * Test issuance: `${your-organisation-wallet-url}/test/issue`
    * Test verification: `${your-organisation-wallet-url}/test/verify`

## Verification Template Enhancement

This enhancement transforms how organisation administrators work with Verification Templates, delivering improvements that greatly simplify management of these templates. Whether you're rapidly prototyping new verification flows or managing complex credential verification scenarios, these updates make the entire process faster and more intuitive.

### Improved visibility & usability

The verification template viewing experience has been significantly improved with a streamlined interface that displays template details in a convenient right-hand side panel when you click a template or select "View", eliminating the need to open new pages. The redesigned structure makes information much easier to locate and understand, while providing both a clear overview and detailed JSON representation of the requested credential for each verification template.

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement01.png" alt="Side pane opens for verification details">

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement02.png" alt="Shows Basic overview of the verification template">

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement03.png" alt="Shows JSON of the verification template">

### Faster template creation
Create templates directly from credential template available within the organisation—ideal for rapid testing and PoC scenarios.

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement04.png" alt="Create verification template using required credential(s)">

Build templates using an improved JSON editor with better usability and built-in base templates.

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement05.png" alt="Create verification template using JSON Editor">

### Full template management

Administrators now have comprehensive template management capabilities at their fingertips. You can edit verification templates directly from the portal, making real-time adjustments to your verification requirements without leaving the interface. Additionally, the duplicate functionality allows you to quickly create new template variations based on existing ones, significantly speeding up the development process when you need similar templates with minor modifications.

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement06.png" alt="Edit and duplicate button for verification teamplate">

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement07.png" alt="Edit verification template page">

### Better Search & Filtering

Filter Presentation Definitions by updated-at-from and updated-at-to for improved navigation and auditing.

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_Portal_Verificarion_Template_Enhancement08.png" alt="Filtering by updated date for verification teampltes">


## Ad-hoc Credential Issuance and Verification

This release adds two new endpoints to the Organisation Wallet, enabling organisations to issue and verify credentials directly, without requiring full OpenID4VCI or OpenID4VP flows.

#### Ad-hoc Credential Issuance

Issue signed credentials programmatically using a new protected POST /credentials endpoint. This feature is ideal for automation, backend integrations, and streamlined testing workflows. It uses the same credential configurations and signing keys available during issuance using OpenID4VCI.

#### Ad-hoc Credential Verification

Verify arbitrary credentials using the new POST /credentials/verify endpoint, which supports multiple credential formats.


## Updated Test Issuance and Presentation UI

The Organisation Wallet's test UI for issuance and presentation of credentials has been redesigned with a modernised interface that displays more relevant test information and supports both **light** and **dark** modes. While no functional changes were made, these updates significantly improve visibility and testing efficiency for developers.

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_TestUI_Enhancement01.png" alt="Updated UI for issuance test UI">

<img src="/.gitbook/assets/releases/3.2.0/Release_3.2.0_TestUI_Enhancement02.png" alt="Updated UI for verify test UI">



# Component Updates


## Portal

### Self-Service Organisation Wallet Deployment (Beta)

New Features:

- Added "Deploy Organisation Wallet" button to `View Application` page, protected by `vc:org:manage` security rights, allowing administrators to initialise a new organisation wallet instance for their application. Button triggers organisation passphrase verification before deployment creation and is only visible when no deployment exists for the application.
- Added "Delete Organisation Wallet" button to `View Application` page, conditionally displayed only when an active deployment exists and the deployment's `app_client_id` matches the current application's client ID, ensuring users can only delete deployments they own. Deletion requires passphrase verification for security.
- Added deployment online/offline status badge displaying "Online"/"Offline" based on the `online_at` timestamp field, shown when a deployment exists for the current application.
- Added informational message "Organisation already has active deployment" displayed when a deployment exists for a different application, preventing conflicting deployments.
- Added `organisationWalletDeployment` feature flag to Config.ts interface and configuration files, enabling/disabling all deployment functionality including API calls and UI controls.

### Verification Template Enhancement

New Features:

- Added new `Verification Template` view side panel.
- Added PEX detection for JSON submissions when creating a `Verification Template`
- Created new sample templates for use in `Verification Template` creation
- Added Getting `Started Section` with `Presentation Definition` sample selection and JSON upload
- Added `Edit` and `Duplicate` functionalities for `Presentation Definition`. When duplicating or editing a verification template, the template can only be edited using the JSON editor.
- Added auto-save and auto-load of draft `Presentation Definitions`.
- Added Trusted Authorities to `Presentation Definitions` builder tool.
- Added base filter component.
- Added filter to `Presentation Definitions` table view, can filter on `updated-at-from` and `updated-at-to`
- Added a page that allows the user to edit `Presentation Definitions`

Enhancements:

- Improved error messages in toast notifications when saving or creating a Presentation Definition fails.
- Changed the `PEX` builder into a `DCQL` builder.
- Changed `Verification Template` table columns. (Definition Name / Created On) -> (Updated On / Tempalte Name / Template Type).
- Changed date display for `Verification template` to include HH:MM:SS as well.
- Change `Presentation Definition` to now disable builder tab if `JSON builder` isnt compatible.
- Changed navigation on `Presentation Definition` create/edit to navigate to the newly created/edited template.
- Changed `Presentation Definition` purpose to optional.

Removed:

- Removed old `Verification Template` view single page designs.
- Removed `PEX` builder tool.
- Removed helper text for `Template Name` and `Purpose` when creating a `presentation definition`, replaced with more details in `Template Details`.

### Security Updates

- Changed `@meeco/sdk` version to `8.6.0-beta`.

## SVX API

### Self-Service Organisation Wallet Deployment (Beta)

New Endpoints:

- Added new endpoint `GET /deployments/ow` to get a list of deployed organisation wallets.
- Added new endpoint `GET /deployments/ow/{id}` to get a specific deployed organisation wallets by `id`.
- Added new endpoint `POST /deployments/ow` to create an organisation wallet deployment.
- Added new endpoint `DELETE /deployments/ow/{id}` to delete an organisation wallet deployment by `id`.

New Features:
- Added a new security right `svx:org:deploy_ow` to enable self-service organisation wallet deployment.
- Added support for automated deployment authorization: `GET /automated-deployments/authorize?admin_id=XYZ&org_od=XYZ&agent_id=XYZ`
- Added `organizations` scope and organisation admin access information to ID tokens, when `organizations` scope is used.

### Verification Template Enhancement

- Added `parameters.presentation_template_name` to the response of `GET /openid/presentations/requests`
- Updated `GET /openid/presentations/requests` query parameters: 
    - added date filtering (created_at_from, created_at_to, updated_at_from, updated_at_to) and improved parameter ordering
- Updated query parameter types for `GET /openid/presentations/requests`: 
    - changed 'page' and 'per_page' from number to integer

### Other Enhancements

- Added support for examples and $comment as top-level attributes in credential schemas
- Added optional PostgreSQL TLS support (`postgres.tls`, p`ostgres.cacertfile`). TLS is disabled by default (when omitted). When enabled, TypeORM will use the specified CA certificate for secure connections with server verification.

### Bug Fixes

- Added validation for empty objects in `handleRedisFind`, so it now returns `undefined` when the key is not found in Redis. Now the system returns 400 error correctly in the case the key is not found in the Redis and an empty object is returned.

### Security Updates:

- Upgraded `oidc-provider` to version 9.5.2 in SVX IDP

## Organisation Wallet

### Ad-hoc Credential Issuance and Verification

New Endpoints:

- Added new protected endpoint `POST /credentials` for Ad-hoc Credential Issuance
- Added new protected endpoint `POST /credentials/verify` for Ad-hoc Credential Verification

Configuration Properties:

- Added required `credential_issuer.supported_client_auth_methods` configuration for better control of `token_endpoint_auth_methods_supported authorization` server metadata
- Issuer metadata `credential_signing_alg_values_supported` attributes uses integers to refer to supported algs.

Fixes:

- Fixed key attestation verification. No longer checks for optional iss claim.
- `exp` claim is optional in wallet attestation token.


### Updated Test Issuance and Presentation UI

- Updated the look and feel of the test pages at `/test/issue` and `/test/verify` (previously `/test/present`). No functional changes have been made, rather the pages have been updated to improve testing by showing more (useful) information. The following javascript libraries were added:
    * Monaco editor
    * TailwindPlus Elements (for modal component and mobile menu)
- Updates test pages to support light and dark mode

### SVX Verify

Configuration Properties:

- Added `svx_verify.idp_options` configuration that allows to set custom authorization_request_endpoint per IDP
- Added `presentation_display_mode` configuration to explicitly control identity verification UI on endpoint `POST /identity/verification/:id `via the `presentation_display_mode` cookie.

Enhancements:

- Changed the Samesite to `lax` for the `svx_verify_session` cookie
- Simplified SVX Verify language select to use query parameters

Removed:

- Removed URL check for `success_url` and `return_url` in` POST /identity/sessions` endpoint to allow any URL to be used, such as a custom app URLs.
- Removed `POST /identity/language` endpoint. Use query parameters to change language preference.

Bug Fixes:

- Fixed an issue where the interaction view’s error page failed to render by adding the required footer information to the request.
