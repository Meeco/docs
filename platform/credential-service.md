# Credential Service

Meeco’s Credential Service enables the issuance, request, presentation, verification and revocation of Verifiable Credentials (VCs). It does this by leveraging open standards to facilitate the creation of trust-based, data exchange ecosystems. Once onboarded, Issuers and Verifiers can develop a range of credential services based on secure, cryptographic, tamper-proof data exchanges. Our Credential Service enables trusted Issuers to design and issue custom VCs which can in-turn be verified by trusted Verifiers.

Meeco’s Credential Service is integrated with the Portal and Wallet offerings to support the complete credential life cycle.

## VCs and VPs

The VC data model that we employ has been established as a standard by [W3C](https://www.w3.org/TR/vc-data-model/). Both VCs and Verifiable Presentations (VPs) are included in this standard and it is important that both concepts are understood when exploring VC workflows.

VCs are comprised of one or more tamper-evident claims plus associated metadata, all of which relate to an entity (a natural or legal person, object, place or thing). VCs include digital proof mechanisms that enable verification. One of these mechanisms is digital signatures which are used to prove credential validity. VCs are issued by Issuers to Holders and often contain an Issuer DID that resolves to a DID document. The DID document references a public key in order to check the Issuer’s cryptographic signature, proving the authenticity of the Issuer.

VPs contain one or more VCs and they can be presented by Holders to Verifiers who can then cryptographically verify the authenticity of the issuer.

## Schemas

Credential Schemas are implemented as JSON schemas. These schemas are a type of metadata that provides a standardized way to define the structure and validation rules for verifiable credentials. It serves as a blueprint, allowing applications to ensure that data conforms to a specific structure and set of rules and promotes interoperability.

## Credential Lifecycle

Typically a VC has a lifecycle comprised of the following events:

- **Issuance**: An Issuer issues a signed VC to a Holder who consents to receiving the VC in their digital wallet. There are different modes of issuance:
  - The VC is pushed from an Issuer, where a thorough consent and Issuer verification mechanism is important to protect the Holder from receiving unsolicited content by unknown issuers. These mechanisms are important to avoid the next era of spam.
  - An entity (such as a Holder) can proactively request a VC to be issued to them.
  - Issuance of a VC can be done instantaneously or deferred until a pre-determined time.
- **Verification**: A Holder will present a VC for verification to a Verifier. By implementing the W3C VP format, credentials can be combined to fulfil a request for credentials, and Holders can implement selective disclosure if available.
- **Revocation or suspension**: An Issuer may revoke or suspend a credential and register its new status in a Verifiable Registry, available to Verifiers upon verification. This makes verification of credentials faster and more accurate. Verifiable Registries can be hosted in a number of different ways:
  - Centralised (e.g. via provider)
  - Decentralised (e.g. using a distributed ledger)
