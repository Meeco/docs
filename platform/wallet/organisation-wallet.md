The Organisation Wallet (OW) is a component of Meeco’s SVX Platform that enables organisations to issue, verify and manage Verifiable Credentials (VCs). The OW is accessible via the API with the same name. The OW is built on international standards and specifications that enable interoperability across VC ecosystems. Organisations can integrate the OW into existing systems and processes and can securely store VCs with Meeco’s secure storage offering, the Vault. The OW’s cryptographic keys are also managed by Meeco’s proprietary key management service, the Keystore.

## Accessing an Organisation Wallet

To access an OW a user must first sign up to Meeco’s SVX Platform. After receiving SVX login credentials, access to the OW API will be granted. When setting up the OW, the creation of an Application will be required. See the [Machine-2-Machine Communication](./machine-2-machine-communication.md) guide for API access or the [Applications](./portal-tutorials/organisation-adminsitrators/applications.md) guide if using the Portal.

## Verifiable Credentials (VCs)

Built on the international specifications [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OpenID Connect for Verifiable Presentations (OID4VP)](https://openid.net/specs/openid-connect-4-verifiable-presentations-1_0-07.html) the OW issues and verifies VCs based on the [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model-2.0/). As more organsiations and ecosystem operators adopt these specifications and standards, VC issuance, verification and Holder engagement becomes easier to initiate across platforms.

Included in the OW offering is the creation and management of credential schemas and credential types. This ensures Issuers and Verifiers have a common understanding of the VCs they are interacting with. It ensures that VCs being issued and verified are aligned with the same specifications and can be recognised by all parties within the ecosystem.

## Use Case Application

As the OW is delivered as an API, its application in use cases is endless. Multiple OWs can be used in a use case to differentiate Issuers and Verifiers, and the workflows they undertake. Additionally, as the types of VCs that can be issued and verified are vast, use cases extend to all disciplines and sectors.

As the OW is delivered as an API, the development of a UI is possible. This UI can be delivered in the form of a browser extension, desktop app, or mobile wallet app. It can also be integrated into existing databases, records management systems as well as standalone devices, including verification systems.

## Diagram

The diagram below provides a high-level overview of how the OW can interact with different services.
