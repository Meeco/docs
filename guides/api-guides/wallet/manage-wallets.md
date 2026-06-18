# Manage Wallets and Keys

A wallet is the container the SVX Wallet manages on behalf of a holder — a collection of cryptographic keys, [DIDs](../dids.md) and credentials. This guide covers the lifecycle of a wallet and of the keys it holds.

All requests require a `Bearer` access token — see [Using the Wallet API](using-the-api.md).

## Wallets

### Create a wallet

A wallet is identified by an `external_reference` you choose — typically a stable identifier for the user or integration the wallet belongs to.

**Endpoint**

```bash
POST /wallets
```

**Request**

```json
{
  "external_reference": "wallet-240201"
}
```

**Response**

The new wallet is returned with its system-generated `id` and its (initially empty) `keys` and `dids`.

```json
{
  "wallet": {
    "id": "e9f0451b-27a0-4db1-bc7a-ae6dd176a456",
    "external_reference": "wallet-240201",
    "keys": [],
    "dids": []
  }
}
```

> **Note**
> _Find-or-create:_ this endpoint is idempotent on `external_reference`. If no wallet exists for the reference, a new one is created and returned with `201 Created`. If a wallet already exists for that reference, the existing wallet is returned with `200 OK` rather than creating a duplicate.

### View a wallet

Retrieve a wallet by its `id`, including the keys and DIDs it currently holds.

**Endpoint**

```bash
GET /wallets/{walletId}
```

**Response**

```json
{
  "wallet": {
    "id": "e9f0451b-27a0-4db1-bc7a-ae6dd176a456",
    "external_reference": "wallet-240201",
    "keys": [
      {
        "kid": "lA5y6IC-aLCpHtf-NytvSbYGZWqAfbrgAoGn3F3l0NI",
        "alg": "ES256",
        "kty": "EC",
        "crv": "P-256",
        "x": "LJJiy0X4NtQAI67uLsr883v0aFEk81LoxJ7xoJwfwyk",
        "y": "mZXpWtIIj3137AjvIeFuAd5g5bE6crBizyloOBFu9ek"
      }
    ],
    "dids": [
      {
        "id": "did:key:zDnaerDaTF5BXEavCrfRZEk316dpbLsfPDZ3WJ5hRTPFU2169",
        "keys": [{ "kid": "did:key:zDnaerDaTF5BXEavCrfRZEk316dpbLsfPDZ3WJ5hRTPFU2169#zDnaerDaTF5BXEavCrfRZEk316dpbLsfPDZ3WJ5hRTPFU2169" }]
      }
    ]
  }
}
```

Only the public part of each key is ever returned; private key material never leaves the service.

### Delete a wallet

**Endpoint**

```bash
DELETE /wallets/{walletId}
```

A successful deletion returns an empty `204 No Content` response.

> **Note**
> Deleting a wallet permanently removes it together with **all of its keys and DIDs**. This cannot be undone. Credentials held in the wallet are no longer usable once the keys they are bound to are gone.

## Manage keys

Each key in a wallet is a key pair generated and held by the service. Keys are used to control [DIDs](../dids.md), to bind credentials to the holder, and to sign verifiable presentations. A key is identified by its `kid`.

### Create a key

Generates a new key pair in the wallet.

**Endpoint**

```bash
POST /wallets/{walletId}/keys
```

**Request**

```json
{
  "key": {
    "kty": "EC",
    "crv": "P-256"
  }
}
```

Supported key types:

| `kty` | `crv` |
| --- | --- |
| `EC` | `P-256` |
| `OKP` | `Ed25519` |

**Response**

The public key is returned, including the generated `kid` and the JWA `alg` it maps to (`ES256` for EC / P-256).

```json
{
  "key": {
    "kid": "lA5y6IC-aLCpHtf-NytvSbYGZWqAfbrgAoGn3F3l0NI",
    "alg": "ES256",
    "kty": "EC",
    "crv": "P-256",
    "x": "LJJiy0X4NtQAI67uLsr883v0aFEk81LoxJ7xoJwfwyk",
    "y": "mZXpWtIIj3137AjvIeFuAd5g5bE6crBizyloOBFu9ek"
  }
}
```

### View a key

**Endpoint**

```bash
GET /wallets/{walletId}/keys/{kid}
```

Returns the public key for the given `kid`, in the same shape as the create response.

### Delete a key

**Endpoint**

```bash
DELETE /wallets/{walletId}/keys/{kid}
```

A successful deletion returns an empty `204 No Content` response. Credentials or DIDs bound to the key can no longer be used once it is deleted.

### Import a key

For advanced use, an existing key pair can be imported into a wallet by supplying the full JWK (including its private members) rather than generating a new one.

**Endpoint**

```bash
POST /wallets/{walletId}/keys/import
```

**Request**

```json
{
  "key": {
    "kty": "EC",
    "crv": "P-256",
    "x": "LJJiy0X4NtQAI67uLsr883v0aFEk81LoxJ7xoJwfwyk",
    "y": "mZXpWtIIj3137AjvIeFuAd5g5bE6crBizyloOBFu9ek",
    "d": "…"
  }
}
```

The imported key is stored like any other and the public key is returned.

### Sign data with a key

For advanced use, a wallet key can sign an arbitrary payload directly. Most callers do not need this — credential binding and presentation signing happen automatically as part of the relevant flows.

**Endpoint**

```bash
POST /wallets/{walletId}/keys/{kid}/sign
```

**Request**

```json
{
  "data": "<base64url-encoded bytes to sign>"
}
```

**Response**

```json
{
  "signature": "<base64url-encoded signature>"
}
```

## Related

* [DIDs](../dids.md) — create and use DIDs derived from a wallet's keys.
* [Add Credentials to a Wallet](add-credentials.md) — receive or import credentials into a wallet.
* [Credentials](../credentials/README.md) — work with credentials once stored.
