# SVX 2.0.0 Release Notes

**Software Release Date:** 06 January 2025

**Summary:** 

In this release, our biggest yet, we’ve focused on evolving the SVX platform to provide the best possible support for organisations looking to issue and verify, and provide wallets for digital credentials in an interoperable, secure and privacy preserving way. We are excited to announce the release of two new components as well as a complete revision of the SVX API to align with the latest global and industry standards, ensuring we maintain a high level of compliance, security, and interoperability.

Here’s what’s new in a nutshell:

**Release of Organisation Wallet and Holder Wallet**  

SVX has evolved to include two new components: Organisation Wallet and Holder Wallet services. Managing the functionalities associated with Issuers, Verifiers and Wallet Providers via separate APIs helps our customers streamline credential issuance, management, and presentation workflows. Built on open standards, these APIs currently support two of the leading credential formats: W3C Verifiable Credentials (JWT) and IETF SD-JWT VCs. 

These new components enhance efficiency, ensure security, and enable seamless integration with global credential ecosystems, empowering users to adopt advanced digital identity solutions with confidence and trust.

**Revised Digital Credentials APIs**

This release features a complete revision of all APIs related to digital credential management, aligning them with the latest changes in global standards in the same domain. These changes include support for: 

- OpenID for Verifiable Credential Issuance (OpenID4VCI) draft 13 (ID-1) 
- OpenID for Verifiable Presentations (OpenID4VP) protocols draft 18 (ID-2)
- W3C Bitstring Status List

Based on our experience gained and the feedback we have gathered in the past couple of years, we have implemented the following revisions in our APIs:

- The consistent application of credential schemas across various credential formats.
- The enablement of credential types to be format-specific to provide greater control over format-specific configuration.
- The management of how presentation requests and responses are handled.
- Consistent issuer and holder key binding for JWT based credential formats.

The APIs have been refactored for consistency and efficiency, including route updates (e.g., replacing `/oidc/` with `/openid/`) and payload restructuring to simplify integration. The HCW API now handles multi-credential submissions more effectively, and the OCW API introduces configurable protocol versions for issuance and presentation workflows.

These improvements streamline credential issuance, presentation, and verification processes while ensuring robust security and adherence to global standards. By embracing cutting-edge specifications and modern credential formats, the SVX platform positions itself as a leader in scalable, interoperable, and future-ready digital identity solutions.

## Organisation and Holder Wallet
As digital identity management, age verification, and other verification-driven use cases continue to grow, organisations - including identity providers and businesses relying on personal data - along with end-users, need a secure and trusted solution for efficient credential management. SVX is excited to introduce our Organisation Wallet and Holder Wallet services, designed to simplify and enhance these complex interactions. They are built on top of the existing SVX API and Portal and leverage credential and secure storage capabilities.

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_SVX_Overview.png" alt="SVX Overview Image." width="80%">
</p>

Both components offer the necessary tools for customers looking to participate in the Issuer-Holder-Verifier model using digital credentials as a mechanism to exchange Issuer asserted claims about the Holder.

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_Data_Model.png" alt="Data Model Image." width="80%">
</p>

### Organisation Wallet Services
Composed of the following tools to bootstrap issuance and verification of credentials:

- Organisation Cloud Wallet (OCW) service for streamlining issuance and verification of credentials for organisations.
- Issuer / Verifier websites integrated with OCW for jumpstarting use cases.

### Holder Cloud Wallet Services
Composed of the following tools:

- Holder Cloud Wallet (HCW) service for streamlining key management, DID management, and the issuance and presentation of credentials.
- White-label Holder Wallet Front End; a multi-platform mobile app, integrated with the HCW service.
- Out-of-the box integration with OAuth 2.0 & OpenID Connect compatible IdPs for user authentication.

### Benefits of Organisation and Holder Cloud Wallet Services
**1. Comprehensive Credential Management:**
- Organisation Wallet: Enables seamless issuance, storage, and presentation of digital credentials for efficient identity operations.
- Holder Wallet: Allows end-users to receive, claim, and present Verifiable Credentials with ease.

**2. Interoperability Across Ecosystems:**
- Both wallets are built on widely accepted standards, including but not limited to OpenID for Verifiable Credential Issuance (OpenID4VCI), OpenID for Verifiable Presentations (OpenID4VP) and IETF SD-JWT VC.
- They enable interoperability across various credential ecosystems unlocking valuable use cases.

**3. Enhanced Security and Trust:**
- Organisation Wallet: Protects sensitive identity data and ensures the authenticity and integrity of issued credentials.
- Holder Wallet: Safeguards individual user data while ensuring the validity of presented credentials.

**4. Scalable Integration:**
- Both services offer APIs that enable organisations, companies, and individuals to integrate seamlessly into existing credential ecosystems, ensuring broad scalability and ease of adoption.

**5. Support for Various Credential Formats:**

The APIs enable the issuance, presentation, and verification of multiple credential formats, including:
- W3C Verifiable Credential Data Model (VCDM) JWT
- IETF SD-JWT VC

**6. Future-Ready Digital Identity Solutions:**
- Both wallets provide a strong foundation for adopting advanced technologies like Verifiable Credentials and decentralised identity frameworks, supporting a wide range of use cases and future innovations.

**7. Streamlined Data and Workflow Management:**
- Organisation Wallet: Simplifies the administration of credential types and linked records, reducing complexity.
- Holder Wallet: Offers a user-friendly mechanism for managing credentials, ensuring smooth workflows for end-users.

**8. Backend Services Designed for Efficiency, Privacy, and Security:**
- Both the Organisation and Holder Wallets are backend services, purposefully designed to prioritise efficiency, privacy, and security in managing credentials.
- The APIs empower SVX customers to develop customised, robust front-end solutions while leveraging global frameworks to ensure seamless interoperability across ecosystems.
- Both services integrate with existing OpenID & OAuth 2.0 compatible IDPs for identity and access management.

## Revised Digital Credentials APIs
To recap, the revision of our Digital Credentials API has led to the following outcomes:
- The consistent application of credential schemas across various credential formats.
- The enablement of credential types to be format-specific to provide greater control over format-specific configuration.
- The management of how presentation requests and responses are handled.
- Consistent issuer and holder key binding for JWT based credential formats.

### Improved Credential and Verification Templates
- When creating Credential Templates, Organisation Administrators can now choose from either `jwt_vc_json` or `vc+sd-jwt` credential formats. This results in format-specific templates that allow customisation of format specific features. For example, when creating a `vc+sd-jwt` credential template, it is now possible to specify disclosure frames associated with selectively disclosable credentials.
- Organisation Administrators have more control when creating verification templates. They can add multiple verifiable credentials of both types (`jwt_vc_json` and `vc+sd-jwt`) to a single verification request and can define the associated options as specified in DIF Presentation Exchange per required credential. See video below for reference.

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_Verification_Template_Creation.gif" alt="Data Model Image." width="80%">
</p>

- **Credential and Verification Templates:**
  - Added the option to select either `jwt_vc_json` or `vc+sd-jwt` credential format when creating a credential template to increase specificity and to enable more use cases.
  - Added a field for specifying disclosure frames when creating `vc+sd-jwt` credential templates to increase control over `vc+sd-jwt` credential issuance.
  - Added a Format column to the credential templates view screen for improved organisation.
  - Added new `credential name` and `format` fields (per required credential) when creating verification templates for enhanced customisation and usability.
  - Made `format algorithm` and `Constraints` _required_ fields when creating verification templates to extend customisation over each required credential.

- **Request Responses:**
  - Added the date a request response was archived (when viewing its details) to enable more accurate record keeping. The associated attribute is `archived on`.

## Consistent Key Binding for Issuer and Holder
- It is now possible to reference a cryptographic key or DID controlled by the Holder with all JWT based formats in a consistent manner. This is done by using the `cnf` claim, introduced in [Proof-of-Possession Key Semantics for JSON Web Tokens (RFC7800)](https://datatracker.ietf.org/doc/html/rfc7800).

Below is an example of an unprotected payload of a JSON Web Token demonstrating how key binding works when using the JWK member to reference the public key part of an asymmetric keypair.

```
{
  "cnf": { 
    "jwk": {
      "kty": "EC",
      "x": "tr_F2pPBVmlpysrvPRojMIrI06DiuhXBz8B9idb8R6s",
      "y": "2MlG9HNZfFZFB6Tv7lUB6KZLaBh2YF77yU8WzPswmqc",
      "crv": "P-256",
      "alg": "ES256"
    }
  }
}
```

Below is an example of an unprotected payload of a JSON Web Token demonstrating how key binding works when using the JWK member referencing a DID URL using the `kid` of the JWK member as well as including the public key of an asymmetric keypair controlling the DID.

```
{
  "cnf": { 
    "jwk": {
      "kid": "did:key:base64#key1-id",
      "kty": "EC",
      "x": "tr_F2pPBVmlpysrvPRojMIrI06DiuhXBz8B9idb8R6s",
      "y": "2MlG9HNZfFZFB6Tv7lUB6KZLaBh2YF77yU8WzPswmqc",
      "crv": "P-256",
      "alg": "ES256"
    }
  }
  "credentialSubject": {
    "id": undefined 
  }
}
```

Note that in the latter case the `id` property of the `credentialSubject` claim remains undefined to not cause clashes.
- Similarly to the point above, an Issuer can be both a HTTPS URL or a DID for all JWT based credential formats (`jwt_vc_json`, `vc+sd-jwt`). 

## Support for X.509 Certificate Chains
- Credentials and Presentation Requests can now be secured using X.509 Certificate Chains. These use the `x5c` header claim, which is a list of X.509 Certificates, that enables verification of the JWT's digital signature. This mechanism ensures the authenticity and integrity of the JWT, helping to prevent token abuse and foster trust between parties. While this component validates the correctness of the signature, it is at the discretion of the SVX consumer to configure and implement trust checks.

## Updated JSON Schema Support
- **Schema Update:** VC API now supports the latest version of the [VC JSON Schema specification](https://www.w3.org/TR/vc-json-schema/). The schema now describes the entire credential, not just credentialSubject as in earlier implementations. For W3C VCDM credentials, credentialSubject must now be explicitly included in the schema.
- **Backward Compatibility:** The verification process continues to support older formats based on the credentialSchema.type attribute. Supported types include:
  - JsonSchemaValidator2018 (current implementation)
  - JsonSchema2023, JsonSchema, and FullJsonSchemaValidator2021 (latest spec versions)
  - Unknown formats will result in errors
- **Schema Usage:**
  - Credential generation no longer includes `credentialSchema`, as it is rarely used in the VC ecosystem.
  - Schemas are used internally to validate if provided data matches the schema during credential generation.
  - Schemas may include reserved attributes (e.g., exp, issuanceDate), though it is recommended to focus on user claims. Reserved claims are respected and customisable if included.
- **Credential Formats:** Different credential formats may require distinct schemas.

This update ensures compliance with the latest specifications and maintains compatibility with legacy formats.

## Various
### UX enhancements
- Added copy buttons to the `Organisation ID` field in Organisation Account Settings, and `Application Domain` field when viewing Applications' details for improved user convenience.

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_Organisation_ID_Field.png" alt="Organisation ID Field Image." width="80%">
</p>

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_Application_Domain_Field.png" alt="Application Domain Field Image." width="80%">
</p>

- Corrected the spelling of `Documentations` to `Documentation` on Tenant and Organisation navigation menus.
- Changed Organisation navigation menu item name and associated screens from `Issue / Revoke Credentials` to `View credentials` as credential issuance functionality has been removed from the Portal (for more information see the [Deprecations and EOL](#deprecations-and-EOL) section).

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_View_Credentials.png" alt="View Credentials Image." width="80%">
</p>

- Removed the `Issue Credential` action item as seen in the item dropdown menu via the `Credential Templates` page. This is due to credential issuance functionality being removed from the Portal (for more information see the [Deprecations and EOL](#deprecations-and-EOL) section).

<p align="center">
<img align="center" src="/.gitbook/assets/Release_2.0.0_Verification_Templates.png" alt="Verification Templates Image." width="80%">
</p>

- The View Verification Request and View Response pages now display either `Wallet DID` or `Issuer URL` as a way to identify either the wallet or issuing organisation. This change was implemented due to `sd+vc-jwt`s not containing a `DID` resulting in an empty field in the Portal. 

### SDK Update
- Upgraded `meeco/sdk` to version `7.8.0-beta` for access to the latest features including those offered by the VC API V2 update.
- Updated `meeco/sdk` to include missing filter properties when creating a verification template. Previously, filter properties including `number` and `arrays` were not included resulting in verification templates to error.

## In summary
- **Bitstring Status List Support:** Enabled issuing and verifying revocable credentials using the new [W3C Bitstring Status List specification](https://www.w3.org/TR/vc-bitstring-status-list/). This is a new specification that enables the publishing of status information such as suspension or revocation of Verifiable Credentials using bitstrings in a privacy-preserving and performance-enhancing way.
- **Schema Improvements:** Added support for a newer version of [W3C JSON Schema](https://www.w3.org/TR/vc-json-schema/). Introduced greater schema definition flexibility so both credential formats: `jwt_vc_json` and `vc+sd-jwt` can be described independently.
- **Credential Types/Templates:** Format configuration introduced to the credential type entity. As credential formats have different requirements and configurations, keeping the credential type entity isolated makes its management easier.
- **Presentation Definitions/Templates:** API extended to support majority of [DIF PEX v2.1.0](https://identity.foundation/presentation-exchange/spec/v2.1.0/). Instead of providing only Credential Type IDs that are expected by the Presentation Definition, Organisation Administrators have to provide a full Presentation Definition structure with the required constraints.
- **New API Features:** Introduced endpoints and attributes for improved credential generation, presentation requests, and payload validation.
- **Streamlined Responses:** Simplified payloads and response structures for better clarity and alignment with updated standards.
- **Interoperability Updates:**
  - Adopted OpenID4VP [version draft18](https://openid.net/specs/openid-4-verifiable-presentations-1_0-18.html) (Implementors Draft 2).
  - Adopted OpenID4VCI [version draft13](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-13.html) (Implementors Draft 1).
  - Integrated JSON Schema validation as per [DIF PEX v2.1.0](https://identity.foundation/presentation-exchange/spec/v2.1.0/).

# New Functionality
## Portal
- Portal users can now experience the UI in Japanese.
- The library of credential schema examples available when creating new credential schemas has been updated to align with VC API V2 changes. These updates offer users enhanced schema capabilities.

# Bug Fixes
## Portal
- Fixed route on the “Issue / Revoke credentials” page's table when clicking on the `VC ID` cell. The user was previously taken to a 404-error page but is now taken to the “View credential” page.
- Fixed an issue where an empty error message would appear after creating a new verification template. An appropriate error message now appears.
- Fixed breadcrumb routing on the “View Tenants” page. Previously, a 404-error page was presented when clicking on the “Tenants” link, now, the user is taken to the list of Tenants.
- Fixed an issue where, when viewing an issued credential’s details, the credential attributes were displayed out of order compared to their arrangement in the associated credential schema. Attributes are now displayed across the platform in the same order as specified by the credential schema.
- Fixed the search functionality to ensure the component would not break when a user entered a search query too quickly. This has made the search component more robust when accepting search queries.
- Fixed default `limit signature algorithm` for verification requests. The algorithm has been changed from RS256 to ES256 as this is the algorithm SVX currently uses.
- Corrected error handling that resulted in a crash for `Validate Schema` when parsing a credential schema. This ensures that users no longer experience a software crash when editing a credential schema.
- Fixed the response status on both `View Verification Request` and `View Request Response` pages as some responses were showing `failed` even if the credential(s) had successfully been verified. The correct response now appears.
- Fixed `Create Credential Schema` to allow empty `Arrays` and `Number` types. These additional schema properties extend the user’s schema capabilities. Prior to this fix, empty `Arrays` and `Number` types would result in an error when creating a new credential schema.
- Fixed errors on `View credential` page when viewing a credential that had been issued using V2 of the VC API. Users can now view all issued credentials without errors occurring.

## SVX API
- Implemented comprehensive monitoring of AMQP configuration and connections to RabbitMQ across the platform. This enhancement enables early detection of unhealthy RabbitMQ states, addressing a critical issue where incomplete propagation of changes (e.g., tenants, orgs, apps) caused disruptions and required complex fixes.

# Performance Improvements
## SVX API
- Horizontal scaling enabled for Authorisation, Tenant, and Organisation Manager (ATOM) used to authorise incoming requests.

# Security Updates
## Portal
- Disabled gzip (except for css) to address web security vulnerabilities.

## SVX API
- KrakenD EE upgraded to version 2.7.6
- Node in Identity Network upgraded to version 20.13.1
- Node in VC upgraded to version 20.16
- Node in IDP upgraded to version 20.16
- Erlang upgraded from version 26.2.5.5
- Ruby upgraded to 3.3.6

# API Changes
## Added
- Added support for `BitstringStatusList` [specification](https://www.w3.org/TR/vc-bitstring-status-list/).
- New revocable credentials will be issued with `BitstringStatusList` status list. Previously issued credentials will remain using `StatusList2021`.
- Added support for verifying credential revocation status for credentials issued with `BitstringStatusList`.
- Added `format` attribute to the response of `POST /credentials/generate`.
- Added support for `JsonSchema` as per the [W3C JSON Schema specification](https://www.w3.org/TR/vc-json-schema/).
- Added Credential schema verification check for `vc+sd-jwt` credential verification. Now, if `CredentialVerificationCheck.SCHEMA` is included in the checks to perform, the schema will be verified against the credential.
- Added Optional properties `$defs`, `name`, `required`, and `additionalProperties` to `CredentialJSONSchemaPayloadDto` for enhanced schema definition flexibility.
- Added `format` and `config` attributes to `POST /credential_types` endpoints. Updated payload requirements for `PUT /credential_types/:id`.
- Added `GET /credential_types` query param `format` filter with `vc+sd-jwt` and `jwt_vc_json` options.
- Added support for generating Presentation Request based on `OpenID4VP-draft18` at `POST /openid/presentations/requests`.
- Added `credential.disclosure_frame` payload param to the `POST /credentials/generate` endpoint.
- Added JSON Schema validation based on DIF PEX v2.1.0 ([DIF Presentation Exchange](https://identity.foundation/presentation-exchange/spec/v2.1.0/#json-schemas)) for presentation definition creation.
- Added `parameters.presentation_definition` into response param to the `POST /openid/presentations/requests` endpoint.
- Added a new required property `disclosure_frame` to the `config` payload of create credential types when the format is `vc+sd-jwt`.
- Added `POST /generate/validate_payload` endpoint to validate credential payloads without generating the credential.

## Removed
- Removed `organizations` attribute from the `GET /credential_types` endpoint response.
- Removed functionality to capture and store `organization` and `tenant` data in the database on `TenantCreated` and `OrgCreated` RabbitMQ events.
- Removed `organizations` and `tenants` tables.
- Removed `Accept` header use to determine `format` of the credential to generate via `POST /credentials/generate`.
- Removed `credentialSchema` from the generated credential token via `POST /credentials/generate`.
- `GET /schemas/:id/:version/schema.json` returns unwrapped schema for `JsonSchema2023` type schemas and response is unchanged for schemas of type `JsonSchema2018`.
- Removed support for generating Presentation Request based on `oidvp-draft10` at `POST /openid/presentations/requests`.
- Removed `POST /openid/presentations/token` due to the removal of `draft-10` support. The `id_token` now refers to the simplified `SIOPv2` specification.
- Removed Verification Request Submissions endpoints:
  - `POST /openid/presentations/requests/:id/submissions`
  - `GET /openid/presentations/requests/:id/submissions`
  - `PATCH /openid/presentations/requests/:id/submissions/:subId`
  - `DELETE /openid/presentations/requests/:id/submissions/:subId`
- Removed `POST /openid/presentations/request/verify` endpoint. The separate verification of the presentation request is no longer supported.
- Removed `GET /openid/presentations/request/:id/jwt` endpoint. VC API no longer stores a signer copy of the request. Client is responsible of hosting it if needed.
- Removed `PATCH /openid/presentations/requests/:id` endpoint. VC API no longer stores a signed copy of the request.
- Removed `tokens` from the Presentation Request response structure.
- Removed `status` from the Presentation Request response structure.
- Removed the `id` and `cnf` validation within claims during credential generation.

## Changed
- When a presentation definition is deleted, all presentation requests linked to it are deleted.
- When a credential type is deleted, all credential records linked to it are deleted.
- When a credential is deleted, all credential revocation status records linked to it are deleted.
- When a revocation list is deleted, all credential revocation status records linked to it are deleted.
- `POST /credentials/generate` payload attribute `cnf` moved from `credential.claims.cnf` to `credential.cnf`.
- `POST /credentials/generate` response attribute `credential.unsigned_vc_jwt` renamed to `credential.credential`.
- `POST /credentials/verify` payload attribute `credential.signed_credential_jwt` renamed to `credential.credential`.
- `POST /presentations/generate` response attribute `presentation.unsigned_vp_jwt` renamed to `presentation.presentation`.
- `POST /presentations/verify` payload attribute `presentation.signed_presentation_jwt` renamed to `presentation.presentation`.
- Changed the `POST /schemas` endpoint to accept the new `JsonSchema` format for schema definitions.
- Enhanced schema validation and set `JsonSchema` as the default schema format for schema creation.
- Changed presentation definition creation from using credential schema to `input_descriptors` provided in payload.
- Changed `POST /presentation_definitions` payload attributes:
  - Removed `format`
  - Renamed `required_credentials` to `input_descriptors`
- Changed `POST /presentation_definitions/` response attributes:
  - Removed `format`
  - Removed `presentation_definition_to_schema`
  - Added `input_descriptors`
- Renamed routes to replace `/oidc/` prefix with `/openid/` for clarity.
- Renamed `token_properties` to `parameters` in the presentation request response structure.
- `POST /openid/presentations/response/verify` changes:
  - `request_uri` payload param replaced with the `presentation_defintion`
  - `vp_token` payload param can be passed as a string or an array of strings
  - `vp_token` response param is an array

## Fixed
- Fixed an issue where Tenant or Organisation data was not removed when receiving `TenantDeleted` or `OrgDeleted` event.

# Deprecations and EOL
- It is no longer possible to issue credentials from the SVX Portal. The actions to `Issue Credential` and `Bulk Issue` have been removed from the `Issue / Revoke Credential` page. The removal of credential issuance functionality from the Portal is aligned with the product roadmap whereby the Portal provides organisations with a business dashboard rather than an application that offers transactional functionality. This functionality has been moved to the Organisation Wallet. It remains possible to view issued and revoked credentials via the Portal. 
- It is no longer possible to create verification requests in the SVX Portal and API. This functionality has been moved to the Organisation Wallet. It remains possible to monitor requests via the Portal.
- It is no longer possible for the last remaining Administrator in a Tenant or Organisation to remove themselves as a user of SVX. This is to ensure access to the Tenant or Organisation remains available to the associated enterprise or business.
- Decentralised Meeco Wallet app available in Apple App Store and Google Play Store is superseded by the Holder Wallet.
- Dropped support for OpenID4VP draft 10.
- Generated W3C VCDM JWT (`jwt_vc_json`) credentials no longer include the `credentialSchema` property.
- Credentials can no longer be issued via the Portal. This functionality used item shares from an Organisation’s Vault to a Holder’s Vault. These connections and associated shares are no longer visible via the Portal. This functionality has been replaced by the Organisation Wallet and can be carried out in the following ways:
  - Utilise a tool such as Postman to make direct calls to the OCW API. Users will be able to simulate issuance and verification workflows without needing to integrate into a full application, or
  - Users can use test issuance (`GET /test/issue`) and presentation (`GET /test/present`) web pages in their OCW deployment. These pages display a simplified UI for quick and easy testing.
