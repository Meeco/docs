The Organisation Wallet (OW) is a component of Meeco’s SVX Platform that enables organisations to issue, verify and manage Verifiable Credentials (VCs). The OW is accessible via the API with the same name. The OW is built on international standards and specifications that enable interoperability across VC ecosystems. Organisations can integrate the OW into existing systems and processes and can securely store VCs with Meeco’s secure storage offering, the Vault. The OW’s cryptographic keys are also managed by Meeco’s proprietary key management service, the Keystore.

### Accessing an Organisation Wallet

To access an OW a user must first sign up to Meeco’s SVX Platform. After receiving SVX login credentials, access to the OW API will be granted. When setting up the OW, the creation of an Application will be required. See the [Machine-2-Machine Communication](./machine-2-machine-communication.md) guide for API access or the [Applications](./portal-tutorials/organisation-adminsitrators/applications.md) guide if using the Portal.

### Verifiable Credentials (VCs)

Built on the international specifications [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OpenID Connect for Verifiable Presentations (OID4VP)](https://openid.net/specs/openid-connect-4-verifiable-presentations-1_0-07.html) the OW issues and verifies VCs based on the [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model-2.0/). As more organsiations and ecosystem operators adopt these specifications and standards, VC issuance, verification and Holder engagement becomes easier to initiate across platforms.

Included in the OW offering is the creation and management of credential schemas and credential types. This ensures Issuers and Verifiers have a common understanding of the VCs they are interacting with. It ensures that VCs being issued and verified are aligned with the same specifications and can be recognised by all parties within the ecosystem.

### Use Case Application

As the OW is delivered as an API, its application in use cases is endless. Multiple OWs can be used in a use case to differentiate Issuers and Verifiers, and the workflows they undertake. Additionally, as the types of VCs that can be issued and verified are vast, use cases extend to all disciplines and sectors.

As the OW is delivered as an API, the development of a UI is possible. This UI can be delivered in the form of a browser extension, desktop app, or mobile wallet app. It can also be integrated into existing databases, records management systems as well as standalone devices, including verification systems.

### Diagram

The diagram below provides a high-level overview of how the OW can interact with different services.

<p align="center">
<img align="center" src=".gitbook/assets/Platform_OW_01_Organisation_Wallet_Service_Diagram.png" width="800">
</p>

> **Note**
> As the OW is delivered as an API, all updates and changes are instant. New versions of the API will be released intermittently with associated updates communicated with customers promptly.

## Organisation Wallet Gateway

In front of the OW sits an API Gateway. This Gateway provides an authentication layer to the OW. The software used to create this Gateway is [KrakenD](https://www.krakend.io/) and its purpose is to manage API traffic, implement rate limiting, instill authentication measures, and serve as a control point for managing API requests. This layer can also enforce security policies and provide additional logging and monitoring capabilities.

Endpoints exist in the OW that are required to be publicly available. This is to ensure [OID4VCI](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OID4VP](https://openid.net/specs/openid-connect-4-verifiable-presentations-1_0-07.html) compliance, and for key and metadata lookup. There are also endpoints that must remain private when controlling and creating credential offers. The Gateway handles the associated authentication to only allow authorised requests to call these private endpoints.

## Issuer and Verifier Template Sites

To enable Organisations to undertake actions associated with issuing and verifying credentials in a real-world scenario, template websites have been developed for customer use. These template websites are built using the OW API to enable quick use case deployment. Each of the issuance and verification sites are multi-page sites that facilitate the following workflows:

### Issuance

- User authentication via a login page.
- Option to issue verifiable credentials of various formats (based on what the Organisation has prepared).
- A form to capture the associated Holder’s information.
- The generation of a QR Code (desktop) or deeplink (mobile) to present the credential offer.

### Verification

- Option to verify credentials of various formats (based on what the Organisation has prepared).
- Presentation of a list of attributes required from the Holder.
- The generation of a QR Code (desktop) or deeplink (mobile) to present the request.
- Notification page on successful / unsuccessful verification of credential(s).

These pages can be added to existing user journeys by navigating to and from existing websites. It is also possible to customise the websites' user interface (UI). This includes the addition of custom text, logos, colour themes and feature images.
