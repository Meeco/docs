# Credential Formats

Verifiable credentials (VCs) are digital credentials that use cryptographic methods to prove authenticity and integrity. Different formats for verifiable credentials exist to meet varying needs for security, privacy, interoperability, and implementation contexts:

1. Security and Cryptographic Methods: Different formats support various cryptographic methods, like JSON Web Tokens (JWT) for compact encoding and Linked Data Proofs (LDP) for integrity through digital signatures.
2. Privacy Requirements: Privacy-focused formats, such as SD-JWT, enable selective disclosure, allowing users to share only specific information without revealing all credential data.
3. Ecosystem Compatibility: Some formats, like JWT, are designed to integrate smoothly with existing systems (e.g., OAuth2, OpenID Connect), while others like DIDs support decentralised ecosystems.
4. International and Legal Standards: Formats like the ISO/IEC 18013-5 (for mobile driver’s licenses) and X.509 certificates are used in regulatory contexts, ensuring that credentials meet specific industry standards.
These variations help verifiable credentials serve diverse applications - from decentralised identity verification to government-issued credentials.

## Formats Summarised
### Data (Serialisation) Formats
#### JSON
JavaScript Object Notation (JSON) is a text-based format used for the serialisation of structured data as defined in [RFC8259](https://datatracker.ietf.org/doc/html/rfc8259). It is human-readable and represents data as key-value pairs, utilising JavaScript's object literal syntax.

#### JSON-LD
JSON-LD is a lightweight Linked Data format built on the JSON specification, defned by W3C. It is a machine-readable format that enables applications to start with a single piece of Linked Data and follow embedded links to access related data hosted across different sites on the Web.

#### CBOR
CBOR (Concise Binary Object Representation) [RFC8949](https://www.rfc-editor.org/rfc/rfc8949.html) is a compact, binary data serialisation format optimised for extremely small code size, efficient message size, and extensibility without requiring version negotiation. 

### Credential Formats
#### JSON Web Token (JWT)
JWT [RFC7519](https://datatracker.ietf.org/doc/html/rfc7519) is for encoding verifiable credentials in a compact, URL-safe way. The claims in a JWT are encoded as a JSON object, which can be included as the payload of a JSON Web Signature (JWS) to provide digital signing or integrity protection, or as the plaintext of a JSON Web Encryption (JWE) to ensure confidentiality through encryption.

#### Verifiable Credentials using JWTs (JWT-VC)
JWT VCs are often issued with a digital signature (JWS) for verification. They are popular in ecosystems using OAuth2 or OpenID Connect. It provides a structure for encoding credentials and can be used with a variety of cryptographic methods.

Credential format identifiers:
* `jwt_vc_json`
* `jwt_vc_json-ld`

#### Selective Disclosure for JWTs (SD-JWT)
Selective Disclosure (SD-JWT) format is an extension of JSON Web Tokens (JWT) that enables users to selectively disclose specific claims within a credential, revealing only the necessary information while keeping other details private. This format enhances privacy in credential sharing by allowing holders to disclose minimal, relevant data in a secure and verifiable way.

Credential format identifiers:
* `vc+sd-jwt`
* `vcdm+sd-jwt`

#### Verifiable Credentials using LDP (LDP-VC)
A Linked Data Proof Verifiable Credential (LDP-VC) is a format for expressing verifiable credentials using Linked Data Proofs as the signature mechanism. The credentials are structured as JSON-LD documents, with claims about a subject signed using cryptographic signature suites. This format supports selective disclosure with one of the following algorisms: BBS, CL-Signatures (CL) or Short Randomizable Signatures (ps-sig).

Credential format identifiers:
* `ldp_vc`

#### Mobile Document (mdoc)
A Mobile Document (mdoc) is a digitally signed, standardised electronic document. It uses the CBOR formatting defined in [RFC 8949](https://www.rfc-editor.org/rfc/rfc8949.html). CBOR uses binary encoding which features a small footprint (especially compared to JSON) and is, therefore, ideal for bandwidth constrained environments. Similar to other formats it can be used for both online and proximity use cases, although it is a better fit for the latter. These documents are designed to securely replace physical credentials including, but not limited to IDs, driving licences, passports, or medical records.

##### Mobile Driver’s Licences (mDLs)
Mobile Driver’s Licences (mDLs) are a type of mdoc.  
Our platform supports the issuance of mDL (and other document types) through OpenID 4 Verifiable Credential Issuance (OID4VCI). We also support ISO/IEC TS 18013-7 using OpenID 4 Verifiable presentation (OID4VP), which enables the presentation of mdocs to a reader (verifier) over the internet.

Credential format identifiers:
* `mso_mdoc`

#### X.509 Certificates
Traditional digital certificates that can be adapted for use as verifiable credentials. X.509 certificates are commonly used in public-key infrastructure (PKI) systems and can work with verifiable credentials by incorporating the VC Data Model.

#### Open Badges 
Open Badges is a format for digital badges, designed to represent and share achievements or credentials. Open Badges can be made cryptographically verifiable, and its core vocabulary allows issuers to express claims in a structured JSON-LD format.

## Comparative Matrix
This matrix compares key features the different credential formats offer.  
|                            |    jwt_vc_json   |         vc+sd-jwt        |              jwt_vc_json-ld              |               vcdm+sd-jwt                |                  ldp_vc                  |                             mso_mdoc                            |
|----------------------------|:----------------:|:------------------------:|:----------------------------------------:|:----------------------------------------:|:----------------------------------------:|:---------------------------------------------------------------:|
| **Platform Supported**     | Supported  [VC DATA](https://www.w3.org/TR/vc-data-model/) | Supported  [SD-JWT-VC v3](https://www.ietf.org/archive/id/draft-ietf-oauth-sd-jwt-vc-03.html) | Not supported To consider in the future. | Not supported To consider in the future. | Not supported To consider in the future. | Online: Supported Offline: Supported via third party technology |
| Credential Format | JWT-VC | SD-JWT | JWT-VC | SD-JWT | LDP-VC | CBOR |
| Data Format | JSON | JSON | JSON-LD | JSON | JSON-LD | CBOR |
| Encoding | Base64 | Base64 | Base64 | Base64 | Base64 | Binary |
| Selective Disclosure (Y/N) | N | Y | N | Y | Y | Y |
| SDO | W3C | IETF | W3C | W3C | W3C | ISO/IEC 18013-5 |
| Best Use Case | Online use cases | Online use cases | Online use cases | Online use cases | Online use cases | Bandwidth constrained environments |

## Additional Information
### Privacy-Enhancing Cryptographic Methods for Verifiable Credentials
#### BBS Signatures
Allows the holder to selectively disclose parts of the credential without revealing all information. This cryptographic method is commonly used in privacy-focused applications.

#### Zero-Knowledge Proofs (ZKPs)
Enable proving certain attributes of a credential without disclosing the actual data (e.g., proving age without revealing birthdate).

### Holder Binding
#### Decentralized Identifier (DID) Authenticated Credentials
This format combines verifiable credentials with decentralized identifiers (DIDs) to verify identity without relying on centralized authorities. The VC Data Model supports DID-based VCs.

#### Proof-of-Possession Key Semantics for JWT
By including the cnf (confirmation) claim in a JWT, the issuer binds the credential to the holder using cryptographic key binding. The verifier can then confirm possession of the key using the method specified in [RFC7800](https://www.rfc-editor.org/rfc/rfc7800.html).

## References
[RFC7519](https://datatracker.ietf.org/doc/html/rfc7519) Jones, M.,  Bradley, J., Sakimura, N., “JSON Web Token (JWT)”, May 2015, RFC 7519: JSON Web Token (JWT).

[RFC7800](https://www.rfc-editor.org/rfc/rfc7800.html) Jones, M.,  Bradley, J., Tschofenig, H., “Proof-of-Possession Key Semantics for JSON Web Tokens (JWTs)”, April 2016,Information on RFC 7800 » RFC Editor.

[RFC8259](https://datatracker.ietf.org/doc/html/rfc8259) Bray, Ed., T., “The JavaScript Object Notation (JSON) Data Interchange Format”,  December 2017, RFC 8259: The JavaScript Object Notation (JSON) Data Interchange Format.

[RFC8949](https://www.rfc-editor.org/rfc/rfc8949.html) C. Bormann, P. Hoffman, “Concise Binary Object Representation (CBOR)”, December 2020, RFC 8949: Concise Binary Object Representation (CBOR).

[ISO.18013-5](https://www.iso.org/standard/69084.html) ISO/IEC JTC 1/SC 17 Cards and security devices for personal identification, "ISO/IEC 18013-5:2021 Personal identification — ISO-compliant driving licence — Part 5: Mobile driving licence (mDL) application", 2021, ISO/IEC 18013-5:2021 .

[JSON-LD](https://www.w3.org/TR/json-ld11/) Kellogg, G., Sporny, M., Longley, D., Lanthaler, M., Champin, P., and N. Lindström, "JSON-LD 1.1: A JSON-based Serialization for Linked Data.", 16 July 2020, JSON-LD 1.1 .

[VC DATA](https://www.w3.org/TR/vc-data-model/) Sporny, M., Noble, G., Longley, D., Burnett, D. C., Zundel, B., and K. D. Hartog, "Verifiable Credentials Data Model 1.1", 3 March 2022, Verifiable Credentials Data Model v1.1 .

[SD JWT](https://www.ietf.org/archive/id/draft-ietf-oauth-selective-disclosure-jwt-14.html) Terbu, O., Fett, D., and B. Campbell, "SD-JWT-based Verifiable Credentials (SD-JWT VC)", Work in Progress, Internet-Draft, draft-ietf-oauth-sd-jwt-vc-18, 15 November 2024, 
Selective Disclosure for JWTs (SD-JWT) .

[SD-JWT-VC](https://www.ietf.org/archive/id/draft-ietf-oauth-sd-jwt-vc-08.html) Terbu, O., Fett, D., and B. Campbell, "SD-JWT-based Verifiable Credentials (SD-JWT VC)", Work in Progress, Internet-Draft, draft-ietf-oauth-sd-jwt-vc-08, 3 December 2024, 
SD-JWT-based Verifiable Credentials (SD-JWT VC) .

[VC DATA 2.0](https://www.w3.org/TR/vc-data-model-2.0/) Sporny, M., Jr, T. T., Herman, I., Jones, M. B., and G. Cohen, "Verifiable Credentials Data Model 2.0", 27 December 2023, Verifiable Credentials Data Model v2.0 .

[VC JOSE COSE](https://www.w3.org/TR/vc-jose-cose/) Jones, M., Prorock, M., Cohen, G., “Securing Verifiable Credentials using JOSE and COSE”, 20 March 2025, Securing Verifiable Credentials using JOSE and COSE .

[SD-JWT-VC-DM](https://github.com/danielfett/sd-jwt-vc-dm) Fett, D., “SD-JWT VC DM Credential Format”,  GitHub - danielfett/sd-jwt-vc-dm: SD-JWT VC Data Model is a credential format that combines the best of both worlds from SD-JWT VC and W3C VCDM .

[OPEN BADGES](https://openbadges.org/) Home | IMS Open Badges.