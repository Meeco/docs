# SVX Supported Standards

Meeco is actively following and where possible contributing in standardisation efforts of the leading groups in the identity, personal data space. This page lists the currently supported standards within the platform.

## Standard Bodies

- [Decentralized Identity Foundation (DIF)](https://identity.foundation/)
- [European Blockchain (EBSI)](https://ec.europa.eu/ebsi)
- [HBAR Foundation](https://hbarfoundation.org)
- [Internet Assigned Numbers Authority (IANA)](https://www.iana.org)
- [Internet Engineering Task Force (IETF)](https://www.ietf.org/)
- [OpenID Foundation (OIDF)](https://openid.net/foundation/)
- [World Wide Web Consortium (W3C)](https://www.w3.org/)

## Standards

| Component | Open Specifications / Standards | Standard Body |
| --- | --- | --- |
| Credential Data Model | [Verifiable Credentials Data Model v1.1](https://www.w3.org/TR/vc-data-model) | W3C |
| Credential Data Format | [JSON Web Token VC (JWT-VC)](https://www.w3.org/TR/vc-data-model/#json-web-token) - signed as JWS ([RFC7515](https://datatracker.ietf.org/doc/html/rfc7515)) | W3C, IETF |
| Credential Presentation | [Presentation Exchange v2](https://identity.foundation/presentation-exchange/spec/v2.0.0/) | DIF |
| Credential Presentation <br/>Transfer Protocol | [OpenID for Verifiable Presentations](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) | OIDF |
| Credential JSON Schema | [Verifiable Credentials JSON Schema 2022](https://w3c-ccg.github.io/vc-json-schemas/) | W3C |
| DID Authentication | [Self-Issued OpenID Provider v2](https://openid.net/specs/openid-connect-self-issued-v2-1_0.html) | OIDF |
| Identifier Data Model | [Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/did-core/) | W3C |
| Entity Identifier (NP) | [did:key](https://w3c-ccg.github.io/did-method-key/) | W3C |
| Entity Identifier (NP,LE) | [did:ebsi](https://ec.europa.eu/digital-building-blocks/wikis/display/EBSIDOC/EBSI+DID+Method) | EBSI |
| Entity Identifier (NP,LE) | [did:hedera](https://github.com/hashgraph/did-method/blob/master/did-method-specification.md) | HBAR |
| Entity Identifier (LE) | [did:web](https://github.com/w3c-ccg/did-method-web) | W3C |
| Revocation | [Verifiable Credential Status List 2021](https://www.w3.org/TR/vc-status-list/) | DIF |
| M2M/User Authentication | [The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749) - Code flow, client credentials flow | IETF |
| User Authentication | [Proof Key for Code Exchange by OAuth Public Clients](https://datatracker.ietf.org/doc/html/rfc7636) - Code flow | IETF |

## Supported Algorithms

### JWS Signature

Following key types are supported for JWS verification. The subset of supported "JWS Algorithms" are part of [IANA - JSON Web Signature Algorithms registry](https://www.iana.org/assignments/jose/jose.xhtml#web-signature-encryption-algorithms).

| JWS Algorithm | Key Type |
| --- | --- |
| ES256 | ECDSA using P-256 and SHA-256 |
| ES256K | ECDSA using secp256k1 and SHA-256 |
| EdDSA | EdDSA using Ed25519 and SHA-256 |

### Master Encryption Key Algorithms

Following algorithms are supported when generating derived keys. Used as defined in [NIST - Master Key](https://csrc.nist.gov/glossary/term/master_key).

| Key Type |
| --- |
| PBKDF2HMAC |

### Key Encryption Algorithms

Following algorithms are supported when encrypting other keys at rest and in transit. Used as defined in [NIST - Key-Encryption-Key](https://csrc.nist.gov/glossary/term/key_encryption_key).

| Key Type |
| --- |
| AES-256-GCM |

### Keypairs

Following keypair algorithms are supported for exchanging keys between parties. Used as defined in [NIST - Key pair](https://csrc.nist.gov/glossary/term/key_pair).

| Key Type |
| --- |
| RSA-4096 |

### Data Encryption Algorithms

Following algorithms are supported when encrypting data at rest and in transit. Used as defined in [NIST - Data Encryption Key](https://csrc.nist.gov/glossary/term/data_encryption_key).

| Key Type |
| --- |
| AES-256-GCM |

## Supported OAuth Authentication Flows

Following flows are supported by the platform.

## OAuth Code Flow & PKCE

The Enterprise Portal uses a code flow in conjunction with Proof Key for Code Exchange (PKCE) for secure authentication of (admin)users logging into the portal.

## Client Credentials Flow

Organisations building services on top of the SVX API can use the Enterprise Portal to create an application to enable machine to machine communication. The application allows access to a Client ID and Secret to perform the client credentials flow. The access token enables organisations to access the resources linked to that organisation.
