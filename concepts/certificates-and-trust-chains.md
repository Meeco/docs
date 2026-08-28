# Certificates and Trust Chains

When a credential or attestation is presented, a Verifier wants to authenticate the Issuer. It is trying to answer the question: *did the organisation that claims to have issued this credential actually issue it?*. *X.509 certificates* is a mechanism that does exactly that. It allows you to bind a public key to a real-world identity, and allow that binding to be traced back to an authority the Verifier has already decided to trust. Certificates are used in various credential specifications, including but not limited to [OpenID4VCI](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OpenID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) flows.

## What a Certificate Is

An X.509 certificate is a signed statement binding a public key to an identity. IIt is a public assertion, made by one party about another, that anyone can independently check.

| Element | Purpose |
|---|---|
| Subject | The identity the certificate describes, as a Distinguished Name (DN) |
| Subject Public Key | The public key being vouched for |
| Issuer | The Distinguished Name of the party that signed this certificate |
| Validity period | The `notBefore` and `notAfter` dates between which it is usable |
| Extensions | Constraints on how the key may legitimately be used |
| Signature | The Issuer's signature over all of the above |

The Subject and public key identify *what* is vouched for. The extensions define the limits of that assertion, and they are what verification software actually enforces:

* **`basicConstraints`**: whether the key may sign other certificates. Authorities are marked `CA:true`; end-entity certificates that sign data but not certificates are `CA:false`. The `pathlen` value limits how many further authorities may appear beneath.
* **`keyUsage`** and **`extendedKeyUsage`**: which cryptographic operations are permitted, and for which application. A certificate used to sign mobile documents must carry the object identifier ISO/IEC 18013-5 defines for that purpose.
* **`subjectKeyIdentifier`** and **`authorityKeyIdentifier`**: identifiers that let software match a certificate to the one that signed it.
* **`subjectAlternativeName` (SAN)**: the identity matched at verification time. In modern practice it is the SAN, rather than the Distinguished Name, that a Verifier compares against the Issuer identifier a credential or assertion asserts.

Extensions are not metadata. A cryptographically valid certificate that carries the wrong extended key usage, or a SAN that does not match the credential's stated Issuer, must be rejected.

## Trust Chains

A **trust chain** is a list of certificates, each signed by the private key belonging to the certificate above it (except for the last one). It allows a Verifier to check if the Public Key present in the target certificate (first certificate in the chain) actually belongs to the subject. As stated previously, the subject can be the issuer of a credential or the presentation requester, or any other assertion used in the different protocols. 

```
  Credential                Leaf certificate           Root certificate
  or request        ◀──── (end entity, CA:false) ◀──── (authority, CA:true)
  signed by the             describes the                self-signed
  leaf's key                signing key                  authority
```

Ecosystems commonly insert one or more **intermediate** authorities between root and leaf, so the root's private key can be held offline and used rarely while day-to-day issuance is delegated. A chain with no intermediates is expressed by setting `pathlen:0` on the root.

Validating a chain means checking, for every link, that the parent's public key verifies the child's signature; that the names and key identifiers line up; that both certificates were within their validity periods; and that the parent was permitted to sign certificates while the leaf is permitted the operation being attempted.

If every link passes, the Verifier has established a chain of cryptographic assertions. What it has *not* established is whether any of it matters.

## Trust Anchors

A **trust anchor** is a public key that a party has decided to trust for reasons outside the certificate system itself. It can be published by a scheme operator, mandated by a regulator, or configured by a platform operator (e.g. Apple, Google, Windows, etc).

The distinguishing feature of an anchor is that its own signature proves nothing. A root certificate is self-signed, so anyone can generate one claiming any organisation's name. It will be internally consistent and completely worthless. What makes a particular root an anchor is that a Verifier deliberately installed it.

This gives a clean division of responsibility:

* **The chain answers a mathematical question**: is this key vouched for, transitively, by that key?
* **The anchor answers a policy question**: and do I consider that key authoritative?

Both are required, and validating a chain without terminating it at a configured anchor is among the most serious implementation errors in credential systems.

A Verifier trusts precisely those Issuers whose chains reach the anchors it trusts. Deciding which anchors to to trust and install is a governance decision, not a technical one, and it is where an [ecosystem's](ecosystems.md) rules are ultimately enforced.

Note that Anchors authenticate organisations that act as Issuer and/or Verifier, but are not generally say nothing about the Holder. Something can be perfectly chained to a trusted root and could still be presented by someone who copied it. Confirming the presenter is the party the credential was issued to uses a different mechanism called [cryptographic holder binding](/platform/verifiable-digital-credentials.md).

### Trust Lists

Self-signed certificates are the usual packaging for an anchor, but only as a convenience — an anchor is really just a name and a public key, and the certificate wrapper simply produces a structure existing tooling can already parse. Nothing requires an anchor to be a root: a party may anchor at an intermediate, trusting one branch of a hierarchy and nothing else.

More importantly, credential ecosystems rarely configure anchors one authority at a time. Membership is governed and changes as Issuers are admitted and removed, and reconfiguring every Verifier for each change does not scale. Instead the acceptable authorities are published together as a **signed trust list**, and what a Verifier configures is the key that signs that list.

This moves the anchor up a level. Individual roots may well be self-signed, but they are not trusted *because* they are self-signed — they are trusted because they appear in a list published by an authority the Verifier already trusts. ISO/IEC 18013-5 works this way: each Issuer Authority Certificate Authority (IACA) is a self-signed root, and Verifiers obtain the acceptable set through a signed list such as the VICAL that AAMVA operates for North American driving licences. Under eIDAS, member states publish signed national trusted lists and the European Commission publishes a signed List of Trusted Lists pointing to them.

A Verifier's trust configuration therefore stays small — often a single list-signing key — while the set of Issuers it accepts may be large and may change without any local change at all.

## Certificates in Credential Issuance

An Issuer holds a signing key whose private half never leaves its key management system. That key is described by an end-entity certificate, commonly called a **Document Signing Certificate (DSC)**, signed by the ecosystem's root authority — in mobile document ecosystems, the IACA that participating Verifiers install as their anchor.

When a credential is issued, the Issuer signs it with that key and embeds the certificate chain in the signed object's header: `x5c` for JWT-based formats such as `dc+sd-jwt`, or `x5chain` for `mso_mdoc`. In both cases the chain is an ordered list of base64-encoded DER certificates, leaf first.

On receipt, the Verifier extracts the chain, validates it up to an IACA it already trusts, confirms the leaf's SAN matches the Issuer identifier the credential asserts, and only then uses the leaf's public key to verify the credential signature. Each check alone is insufficient: a valid signature from an untrusted chain proves nothing about the Issuer, and a valid chain whose SAN does not match proves only that *somebody* in the ecosystem signed the credential.

## Certificates in Presentation Requests

The same machinery runs in reverse when a Verifier requests a credential. Under OID4VP the Verifier signs its presentation request with a dedicated request signing key and attaches that key's chain to the signed request. The wallet validates the chain against its own anchors before displaying anything to the Holder, and under the `x509_san_dns` client identifier scheme the request's `client_id` must match the SAN in the leaf certificate.

Without this, a presentation request is an anonymous demand for personal data. With it, the wallet can name the requesting organisation and decline requests from parties outside the trust framework.

This is why issuance and presentation are normally anchored separately, under different roots. *Who may issue credentials* and *who may request them* are distinct questions, often answered by different governance bodies, and separate anchors keep the two decisions independent.

## Validity and Revocation

**Validity periods nest, and credentials outlive them.** A leaf certificate must sit within the validity window of the authority that signed it, and ecosystem rules typically cap leaf lifetimes well below root lifetimes. Because credentials routinely remain valid after their signing certificate expires, Verifiers check that the certificate was valid *at the time of signing*, not at the time of verification.

**Withdrawal is usually handled by the trust list.** Certificate revocation lists and OCSP see little use here, because republishing the trust list without a compromised or departed authority is faster. This is distinct from revoking an individual *credential*, which Issuers do through a [Verifiable Credential Status List](/platform/verifiable-digital-credentials.md#credential-lifecycle).

## Certificates in SVX

SVX Wallet generates signing keys inside its [key management system](../platform/cryptographic-keys.md) and never exports the private halves, so certificates are obtained by generating a signing request against a named key, having it signed by a root authority, and importing the result. The Wallet distinguishes `managed` certificates — bound to one of its own signing keys and used for outbound signing — from `trust_anchor` and `iaca` certificates, which are imported so that inbound chains can be validated.

For the full lifecycle, see the [Certificates guide](../guides/api-guides/certificates/certificates.md). X.509 is one of three identifier options the Wallet supports for Issuers and Verifiers, alongside URLs and [DIDs](../guides/api-guides/dids.md); see [Credential Capabilities](/platform/verifiable-digital-credentials.md#issuer-and-verifier-identifiers) for the comparison.

## Further Reading

* [RFC 5280 — Internet X.509 Public Key Infrastructure Certificate and CRL Profile](https://datatracker.ietf.org/doc/html/rfc5280)
* [RFC 5914 — Trust Anchor Format](https://datatracker.ietf.org/doc/html/rfc5914)
* [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html)
* [OpenID for Verifiable Presentations (OID4VP)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)
* [SD-JWT-based Verifiable Credentials (SD-JWT VC)](https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/)
* [OpenID Federation 1.0](https://openid.net/specs/openid-federation-1_0.html) — trust chains as signed entity statements rather than certificates
* [AAMVA — Mobile Driver's Licence](https://www.aamva.org/topics/mobile-driver-license) — implementation guidelines and the North American VICAL
* [EU Trusted List Browser](https://eidas.ec.europa.eu/efda/tl-browser/) — national trusted lists and the List of Trusted Lists published under eIDAS
* [EU Digital Identity Wallet Architecture and Reference Framework](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework)
* ISO/IEC 18013-5 — defines the IACA and Document Signing Certificate profiles and the VICAL format. Available for purchase from the ISO catalogue.
