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
- Organisation Administrators have more control when creating verification templates. They can add multiple verifiable credentials of both types (`jwt_vc_json` and `vc+sd-jwt`) to a single verification request and can define the associated options as specified in DIF Presentation Exchange per required credential. See screenshot below for reference.

VIDEO

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
**UX enhancements**
- Added copy buttons to the `Organisation ID` field in Organisation Account Settings, and `Application Domain` field when viewing Applications' details for improved user convenience.



