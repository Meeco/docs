# Decentralised Identifiers (DIDs)

A [Decentralised Identifier (DID)](../../platform/did.md) is a globally unique identifier that can be cryptographically verified without relying on a central authority. In SVX, DIDs are handled by the SVX Wallet: a service that manages wallets, each holding keys, DIDs and credentials, and that also provides the issuance and verification capabilities.

DIDs are optional in SVX. The standard issuance and verification flows do not require them, holder binding is typically achieved with a raw key (`cnf.jwk`), and Issuers / Verifiers are typically identified by a URL or X.509 certificate. However, DIDs remain fully supported: they can be created and held in a wallet, and used as the identifier of an Issuer or Verifier, where an ecosystem requires them.

## Supported DID methods

The SVX Wallet supports the following DID methods:

| Method    | Description                                                                                                          |
| --------- | -------------------------------------------------------------------------------------------------------------------- |
| `did:jwk` | The DID is the base64url-encoded public key (JWK) itself.                                                            |
| `did:key` | The DID is the multibase-encoded public key. An EBSI-compliant variant can be created using the `jwk_jcs-pub` codec. |

Both methods are _static_: the DID is derived directly from a newly generated key pair and stored in the SVX Wallet's encrypted database. No ledger or external registry is involved, and the DID requires no ongoing maintenance. As a consequence, static DIDs cannot be updated or deactivated — they are simply created and, when no longer needed, deleted together with their keys.

Supported key types when creating a DID:

| `kty` | `crv`     |
| ----- | --------- |
| `EC`  | `P-256`   |
| `OKP` | `Ed25519` |

The SVX Wallet can resolve `did:key` (including the EBSI variant) and `did:jwk`.

## Create a DID

Creates a new key pair inside a wallet and derives a DID from it.

**Endpoint**

```bash
POST /wallets/{walletId}/dids
```

**Request**

```json
{
  "method": "key",
  "options": {
    "key": {
      "kty": "EC",
      "crv": "P-256"
    }
  }
}
```

- `method` – the DID method, either `jwk` or `key`
- `options.key` – the key type used to generate the key pair (`kty` and `crv`)
- `options.codec` _(optional)_ – set to `jwk_jcs-pub` to create an EBSI-compliant `did:key`

> **Note**
> The `jwk_jcs-pub` codec is only valid when `method` is `key` and the key type is `EC` / `P-256`.

**Response**

The response contains the DID and a reference to the key(s) it controls. The key ID (`kid`) is the DID followed by the verification method fragment.

```json
{
  "did": {
    "id": "did:key:zDnaerDaTF5BXEavCrfRZEk316dpbLsfPDZ3WJ5hRTPFU2169",
    "keys": [
      {
        "kid": "did:key:zDnaerDaTF5BXEavCrfRZEk316dpbLsfPDZ3WJ5hRTPFU2169#zDnaerDaTF5BXEavCrfRZEk316dpbLsfPDZ3WJ5hRTPFU2169"
      }
    ]
  }
}
```

## Delete a DID

Deletes a DID and all keys associated with it from the wallet. This action cannot be undone.

**Endpoint**

```bash
DELETE /wallets/{walletId}/dids/{did}
```

A successful deletion returns an empty `204 No Content` response.

## DIDs in credential flows

When a wallet requests a credential via [OpenID Connect](openid-connect/README.md) (OpenID4VCI), the holder binding can reference either a DID or a raw key (`kid`) — exactly one of the two:

- For the `jwt_vc_json` format, the DID becomes the `credentialSubject.id` of the issued credential.
- For the `dc+sd-jwt` format, DIDs are not supported as holder binding; the holder's key is embedded as a `cnf.jwk` claim instead.

During presentation (OpenID4VP), the wallet signs the presentation with the key controlled by the DID, and the Verifier validates the signature by resolving the DID to its public key.

## DIDs as Issuer and Verifier identifiers

When the SVX Wallet acts as an Issuer or Verifier, its public identifier (`issuer.identifier` in its configuration) is a URI and may be a DID. When a DID is configured:

- **Issuance** – the DID is used as the `iss` claim of issued credentials, allowing wallets and verifiers to resolve the Issuer's public keys from the DID document. For example: `did:web:did-web.securevalue.exchange:02978ef1-8fea-46ee-82e7-6ab6e96d1633`.
- **Verification** – presentation requests identify the Verifier using the OpenID4VP `decentralized_identifier:` Client Identifier prefix followed by the DID.

If a DID is not configured, the SVX Wallet falls back to URL- or X.509-based identification (`redirect_uri`, `x509_san_dns` or `x509_hash`).
