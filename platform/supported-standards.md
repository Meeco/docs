# SVX Supported Standards

Meeco is actively following and where possible contributing in standardisation efforts of the leading groups in the identity, personal data space. This page lists the currently supported standards within the platform.

## Standard Bodies

- [Decentralized Identity Foundation (DIF)](https://identity.foundation/)
- [European Blockchain (EBSI)](https://ec.europa.eu/ebsi)
- [HBAR Foundation](https://hbarfoundation.org)
- [Internet Engineering Task Force (IETF)](https://www.ietf.org/)
- [OpenID Foundation (OIDF)](https://openid.net/foundation/)
- [World Wide Web Consortium (W3C)](https://www.w3.org/)

## Standards

| Component | Open Specifications / Standards | Standard Body |
| --- | --- | --- |
| Credential Data Model | [Verifiable Credentials Data Model v1.1](https://www.w3.org/TR/vc-data-model) | W3C, IETF |
| Credential Data Format | [JSON Web Token VC (JWT-VC)](https://www.w3.org/TR/vc-data-model/#json-web-token) - signed as JWS ([RFC7515](https://datatracker.ietf.org/doc/html/rfc7515)) | W3C, IETF |
| Credential Presentation | [Presentation Exchange v2](https://identity.foundation/presentation-exchange/spec/v2.0.0/) | DIF |
| Identifier Data Model | [Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/did-core/) | W3C |
| Entity Identifier (NP) | [did:key](https://w3c-ccg.github.io/did-method-key/) | W3C |
| Entity Identifier (NP,LE) | [did:ebsi]() | W3C |
| Entity Identifier (NP,LE) | [did:hedera]() | HBAR |
| Entity Identifier (LE) | [did:web](https://github.com/w3c-ccg/did-method-web) | W3C |
| Revocation | [Verifiable Credential Status List 2021](https://identity.foundation/.well-known/resources/did-configuration) | DIF |
| Wallet Authentication | [Self-Issued OpenID Provider v2](https://openid.net/specs/openid-connect-self-issued-v2-1_0.html) | OIDF |

## Supported Algorithms

### JWS Signature

Following key types are supported for JWS verification.

| Key Type | JWT Algorithm |
| --- | --- |
| Ed25519 | EdDSA |

### Master Encryption Key Algorithms

Following algorithms are supported when generating derived keys.

| Key Type |
| --- |
| PBKDF2HMAC |

### Key Encryption Algorithms

Following algorithms are supported when encrypting other keys at rest and in transit.

| Key Type |
| --- |
| AES-256-GCM |

### Keypairs

Following keypair algorithms are supported for exchanging keys between parties.

| Key Type |
| --- |
| RSA-4096 |

### Data Encryption Algorithms

Following algorithms are supported when encrypting data at rest and in transit.

| Key Type |
| --- |
| AES-256-GCM |

