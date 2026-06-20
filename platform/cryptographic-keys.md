# Keys

Cryptographic key management in SVX is handled directly within the [Wallet Service](/platform/wallet.md). Keys are generated automatically by the Wallet's integrated Key Management System (KMS); they do not need to be supplied or configured by the operator. All cryptographic operations (signing, encryption, decryption) are performed within the KMS module, providing a well-defined boundary around private key material.

## Key roles

Keys are organised by role. Each key has a single, well-defined purpose, making it straightforward to reason about what is signed or encrypted with each key and to rotate individual keys independently:

| Key role | Purpose | Default algorithm |
|---|---|---|
| `CredentialKey` | Signs access tokens during OID4VCI credential issuance | ES256 |
| `AccessTokenKey` | Signs issued Verifiable Credentials; public key exposed via well-known endpoint | ES256 |
| `PresentationRequestKey` | Signs Presentation Request JWT objects (OID4VP) | ES256 |
| `KeyEncryptionKey` | Encrypts Data Encryption Keys (DEKs) used for database encryption | AES-256-GCM |
| `AdminSigningKey` | Signs and verifies access tokens for Wallet Dashboard access | ES256 |

Signing keys support ES256 (default) and EdDSA as alternative algorithms. Each key can be individually configured to use a different KMS adapter.

Two optional keys are generated only when the corresponding feature is enabled:

| Key role | Purpose |
|---|---|
| `HolderClientAssertionKey` | Self-signs a Client Assertion for Holder Wallet client authentication |
| `HolderClientAttestationKey` | Self-signs a Client Attestation for Holder Wallet client authentication |

## KMS adapters

Two adapters are supported. An operator can configure different adapters for different key roles.

### Local KMS adapter

Keys are generated using Meeco's [Cryppo](https://github.com/Meeco/cryppo-js) library and stored in the Wallet database. Private key material is encrypted at rest using a **Master Encryption Key (MEK)**: a 256-bit AES key that the operator supplies in the static configuration file under `kms.master_encryption_key`. Without a configured MEK, key material is stored unencrypted in the database. Data remains encrypted (see [Encrypted storage](#encrypted-storage) below), but the keys protecting it are not. Configuring the MEK is strongly recommended for any environment other than local development.

The Local KMS adapter is suitable for development and single-server environments.

### AWS KMS adapter

Keys are stored in and used directly via AWS KMS. Cryptographic operations such as signing are performed via AWS KMS API calls, so private key material never leaves AWS KMS infrastructure. For database encryption, the adapter calls `GenerateDataKey` to atomically create and wrap a DEK.

The AWS KMS adapter is recommended for production deployments.

## Encrypted storage

Data at rest in the Wallet is protected using **envelope encryption**:

1. At write time, a unique **Data Encryption Key (DEK)** is generated for each record.
2. The record is encrypted with the DEK using AES-256-GCM, which provides both confidentiality and authenticated integrity.
3. The DEK is encrypted by the **Key Encryption Key (KEK)** managed by the configured adapter and stored alongside the ciphertext.
4. The plaintext DEK exists only in memory during the write operation and is never persisted.

At read time, the encrypted DEK is retrieved alongside the ciphertext, decrypted by the KEK, and used to decrypt the record. This approach means that rotating the KEK requires only re-encrypting the stored DEKs, not re-encrypting all data.

## Key registry

A Key Registry stored in the Wallet database records the reference ID and metadata (including the public key) for each managed key. This avoids the need to fetch public keys from the KMS backend on every request; for example, the `AccessTokenKey` public key is served immediately from the registry via the `/jwks` endpoint without a round trip to the KMS backend.

## Cryppo

The Cryppo library (`cryppo-js`) is used internally by the Wallet service for cryptographic operations in the Local KMS adapter path. It is also used internally by the [Vault](/other-products/vault/README.md) (a separate product). For details on the library itself, see the [Cryppo page](/other-products/cryppo.md).
