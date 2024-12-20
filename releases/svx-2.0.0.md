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
