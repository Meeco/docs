# Add Credentials to a Wallet

There are two ways a credential is added to an SVX Wallet:

* **Receive from an issuer** using the OpenID4VCI flow — the wallet acts on a credential offer, obtains an access token, and collects the issued credential. This is the normal, issuer-driven path.
* **Direct import** — an already-issued, encoded credential is stored in the wallet in a single call.

In both cases the credential is stored in the wallet, encrypted at rest, and can then be listed, viewed, presented, or deleted.

## Prerequisites

* An existing **wallet**. A wallet is identified by its `walletId` and is created with `POST /wallets`. See [Manage Wallets](manage-wallets.md).

  ```bash
  POST /wallets
  ```
  ```json
  { "external_reference": "wallet-240201" }
  ```

* A **key** (or [DID](../../platform/did.md)) to bind the credential to the holder. Most credentials are issued against a holder key; create one with `POST /wallets/{walletId}/keys` and note the returned `kid`. See [Manage Keys](manage-keys.md).

  ```bash
  POST /wallets/{walletId}/keys
  ```
  ```json
  { "key": { "kty": "EC", "crv": "P-256" } }
  ```

## Receive a credential (OpenID4VCI)

Receiving a credential from an issuer is a sequence of three calls. Each call returns a `state` value that links the steps of a single flow; pass it to the next call.

### 1. Register the credential offer

The flow begins with a credential offer from an issuer, typically delivered as an `openid-credential-offer://` URI (for example, encoded in a QR code). Register the offer with the wallet to start the flow.

**Endpoint**

```bash
POST /wallets/{walletId}/receive
```

**Request**

```json
{
  "credential_offer_endpoint": "openid-credential-offer://?credential_offer_uri=https://issuer.example.com/credential_offers/cfNTGRqlUMWaj9HQNpnsp4"
}
```

You may instead pass a `credential_offer_uri` directly, or an inline `credential_offer` object, if you already hold the offer.

**Response**

The response echoes the resolved offer and returns the `state` for the flow. The offer's `grants` indicate how to obtain an access token — here a `pre-authorized_code` grant that also requires a 4-digit transaction code (PIN).

```json
{
  "state": "682c0278-da29-4f23-9f1c-165777bed32b",
  "credential_offer": {
    "grants": {
      "urn:ietf:params:oauth:grant-type:pre-authorized_code": {
        "pre-authorized_code": "mSvEkWPgwQ3iOwPJRAYRgN",
        "tx_code": { "input_mode": "numeric", "length": 4 }
      }
    },
    "credential_configuration_ids": ["StudentID_sd_jwt"],
    "credential_issuer": "https://issuer.example.com"
  }
}
```

### 2. Obtain an access token

Exchange the offer for an access token from the issuer's authorization server.

**Endpoint**

```bash
POST /wallets/{walletId}/receive/get_access_token
```

**Request** (pre-authorized_code grant)

When the offer specifies a `tx_code`, supply the PIN the issuer shared with the holder.

```json
{
  "state": "682c0278-da29-4f23-9f1c-165777bed32b",
  "tx_code": "4759"
}
```

**Response**

```json
{
  "state": "682c0278-da29-4f23-9f1c-165777bed32b",
  "access_token": "eyJhbGciOiJFUzI1NiIsInR5cCI6ImF0K2p3dC…",
  "token_type": "bearer",
  "expires_in": 86400,
  "authorization_url": null
}
```

> **Note**
> _Authorization code grant:_ if the offer uses an `authorization_code` grant instead, this call returns an `authorization_url` rather than an access token. Open that URL in a browser, complete the login, and the issuer redirects back to the wallet to continue the flow. The access token is then held against the `state` ready for the next step.

### 3. Collect the credential

Request the credential, binding it to a holder key by `kid` (or to a `did`). The wallet stores the issued credential automatically and returns it.

**Endpoint**

```bash
POST /wallets/{walletId}/receive/get_credential
```

**Request**

```json
{
  "state": "682c0278-da29-4f23-9f1c-165777bed32b",
  "kid": "lA5y6IC-aLCpHtf-NytvSbYGZWqAfbrgAoGn3F3l0NI"
}
```

> **Note**
> Provide exactly one of `kid` or `did`. For the `dc+sd-jwt` format the holder's key is embedded as a `cnf.jwk` claim; `did` is not supported for that format.

**Response**

The credential is now stored in the wallet. `id` is the wallet's identifier for the stored credential, and `credential` is the issued credential in its encoded form.

```json
{
  "id": "b3f1c0e2-1d4a-4b8e-9c2a-7f6d5e4c3b2a",
  "state": "682c0278-da29-4f23-9f1c-165777bed32b",
  "credential_id": "urn:uuid:58864fac-1857-40f8-9f37-8fdd4fe1cc2e",
  "credential": "eyJ0eXAiOiJkYytzZC1qd3QiLCJhbGciOiJFUzI1NiJ9…"
}
```

You can review in-progress and completed flows for a wallet with `GET /wallets/{walletId}/receive`, or a single flow with `GET /wallets/{walletId}/receive/{state}`.

## Import a credential directly

If you already hold an issued, encoded credential, store it in the wallet in a single call — no offer or access token is involved. This is useful when a credential is obtained out of band or migrated from another store.

**Endpoint**

```bash
POST /wallets/{walletId}/credentials/import
```

**Request**

```json
{
  "credential": "eyJ0eXAiOiJkYytzZC1qd3QiLCJhbGciOiJFUzI1NiJ9…",
  "format": "dc+sd-jwt"
}
```

* `credential` – the encoded credential string
* `format` – the credential format. Supported values:

  | Format | Description |
  | --- | --- |
  | `jwt_vc_json` | W3C Verifiable Credential as a JWT. |
  | `dc+sd-jwt` | SD-JWT VC (selective disclosure). |
  | `mso_mdoc` | ISO mdoc / mDL. |

**Response**

The stored credential is returned, including the wallet's `id` for it and parsed metadata such as the issuer and credential type.

```json
{
  "credential": {
    "id": "b3f1c0e2-1d4a-4b8e-9c2a-7f6d5e4c3b2a",
    "format": "dc+sd-jwt",
    "credential": "eyJ0eXAiOiJkYytzZC1qd3QiLCJhbGciOiJFUzI1NiJ9…",
    "created_at": "2026-06-13T09:30:00.000Z",
    "meta": {
      "vct": "StudentID",
      "issuer": "did:web:did-web.securevalue.exchange:02978ef1-8fea-46ee-82e7-6ab6e96d1633",
      "kid": "lA5y6IC-aLCpHtf-NytvSbYGZWqAfbrgAoGn3F3l0NI",
      "credential_id": "urn:uuid:58864fac-1857-40f8-9f37-8fdd4fe1cc2e"
    }
  }
}
```

Once stored, a credential can be listed and retrieved with `GET /wallets/{walletId}/credentials`, presented to a verifier, or removed. See [Manage Credentials](manage-credentials.md).
