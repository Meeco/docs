# Credential Capabilities

SVX provides a complete set of credential capabilities covering the full lifecycle of Verifiable Credentials (VCs): issuance, presentation, verification, and revocation. As of SVX 4.0, these capabilities are consolidated within the unified [Wallet Service](/platform/wallet.md) rather than a separate Credential Service.

Issuers and Verifiers access credential functionality through the Wallet API, and manage their credential workflows through the [Wallet Dashboard](/platform/wallet.md#wallet-dashboard).

## VCs and VPs

The VC data model is established by the [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/). Both Verifiable Credentials (VCs) and Verifiable Presentations (VPs) are part of this standard.

VCs are comprised of one or more tamper-evident claims plus associated metadata, all of which relate to an entity (a natural or legal person, object, place, or thing). VCs include digital proof mechanisms that enable verification. Issuers sign credentials with their private key; Verifiers confirm authenticity by resolving the Issuer's public key (via a well-known endpoint, DID, or X.509 certificate).

VPs contain one or more VCs. A Holder presents a VP to a Verifier, who cryptographically verifies the authenticity of each enclosed credential and the Holder's binding to it.

## Supported credential formats

| Format | Identifier | Notes |
|---|---|---|
| IETF SD-JWT VC | `dc+sd-jwt` | Supports selective disclosure. Holder binding via `cnf.jwk`. |
| W3C VCDM (JWT) | `jwt_vc_json` | W3C VC Data Model encoded as JWT. Supports DID-based holder binding. |
| ISO mDOC | `mso_mdoc` | ISO 18013-5 mobile document format. |

## Schemas

Credential Schemas are JSON Schemas that define the structure and validation rules for VCs. They act as a blueprint ensuring that credential data conforms to a specific structure, promoting interoperability across ecosystems. Schemas are managed by the Wallet service via the `/issuer/schemas` endpoints and can be assigned to Organisations via the Portal.

## Credential lifecycle

A typical VC lifecycle within SVX:

**Issuance:** An Issuer creates a credential template, generates a credential offer (OID4VCI), and issues a signed VC to a Holder. The Holder's wallet receives the credential and stores it encrypted. Issuance can be initiated by the Issuer (push) or by the Holder requesting a credential.

**Presentation:** A Holder presents one or more VCs to a Verifier in response to a Presentation Request (OID4VP). The Holder may apply selective disclosure where supported by the credential format. The Verifier validates the credential signatures and the Holder's binding.

**Revocation and suspension:** An Issuer may revoke or suspend a credential by updating its status in a Verifiable Credential Status List, published via the Wallet service. Verifiers check status lists during verification to confirm a credential remains valid.

## Issuer and Verifier identifiers

The Wallet service supports three types of public identifier for Issuers and Verifiers:

* **URL** — the `app_host` of the Wallet deployment acts as the issuer/verifier identifier, resolved via well-known endpoints.
* **DID** — `did:key` (including EBSI variant) and `did:jwk` are supported. The DID is included as the `iss` claim of issued credentials and used as the Client Identifier in OID4VP presentation requests.
* **X.509** — a certificate paired with a signing key; the certificate's Subject Alternative Name (DNS) is used as the identifier. Trust chains are configured via the certificate management endpoints.

## API reference

Credential-related endpoints in the Wallet API:

| Prefix | Purpose |
|---|---|
| `/issuer/schemas` | Credential schema management |
| `/issuer/templates` | Credential template management |
| `/issuer/offers` | Credential offer management |
| `/issuer/credentials` | Issued credential management |
| `/verifier/templates` | Verification template management |
| `/verifier/requests` | Presentation request management and responses |

For full details see the [API guides](/guides/api-guides/credentials/README.md).
