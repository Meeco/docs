# Wallet Service

The SVX Wallet is a unified service that acts simultaneously as a credential issuer, credential verifier, and holder wallet provider. Prior to SVX 4.0, these roles were spread across three separate deployments: the Organisation Wallet (OW), the Holder Wallet (HW), and the VC Service. They are now consolidated into a single service with a single API surface, a single database, and a single deployment to manage.

## What the Wallet Service does

The Wallet Service handles the full credential lifecycle:

**As an Issuer**, the Wallet Service manages credential schemas, credential templates, and credential offers. It issues Verifiable Credentials to holder wallets via the OpenID for Verifiable Credential Issuance (OID4VCI) protocol. It tracks issued credentials and supports credential status management (revocation and suspension).

**As a Verifier**, the Wallet Service creates and manages verification templates and presentation requests. It receives and validates credential presentations from holders via the OpenID for Verifiable Presentations (OID4VP) protocol. It records presentation responses for audit and reporting purposes.

**As a Holder Wallet provider**, the Wallet Service hosts end-user wallets. Each wallet holds cryptographic keys, DIDs, and verifiable credentials. Wallet holders can request credential issuance and present credentials to verifiers using their wallet.

**As a Bridge**, the Wallet Service supports account-based identity flows using OpenID Connect, enabling credential derivation for connected identity providers.

## API structure

The Wallet API uses a consistent role-prefixed structure to make the purpose of each endpoint clear at a glance:

| Prefix | Purpose |
|---|---|
| `/issuer/*` | Credential schemas, templates, offers, issued credentials, status lists |
| `/verifier/*` | Verification templates, presentation requests, and responses |
| `/wallets/*` | Holder wallet management: keys, DIDs, credentials |
| `/verify/*` | SVX Verify sessions for end-user identity verification flows |
| `/bridge/*` | Bridge/OIDC account flows |
| `/system/*` | Runtime configuration, certificates, settings |
| `/application/*` | Application API key management |
| `/admin/*` | Admin account management |
| `/assets/*` | Image asset storage for credential display |

OAuth Authorization Server endpoints (`/authorize`, `/par`, `/token`, etc.) are not prefixed. Well-known endpoints (`/.well-known/*`) follow the relevant protocol specifications.

## PII containment

A key design principle of the SVX 4.0 Wallet is that personally identifiable information (PII) never leaves the Wallet service boundary. In previous versions, issuing a credential required PII to flow through multiple services: the Organisation Wallet, the VC Service, and the Vault. Each additional service that touches PII expands the compliance perimeter.

In SVX 4.0, all credential data is stored and encrypted within the Wallet service itself. This makes it substantially easier to reason about where sensitive data lives and to demonstrate compliance with data protection requirements.

## Cryptographic key management

The Wallet service has an integrated Key Management System (KMS) that generates, stores, and uses cryptographic keys automatically. Keys no longer need to be supplied via configuration files.

Keys are organised by role, so each key has a single, well-defined purpose:

| Key role | Purpose |
|---|---|
| `CredentialKey` | Signs access tokens during credential issuance (OID4VCI) |
| `AccessTokenKey` | Signs issued Verifiable Credentials; public key exposed via well-known endpoint |
| `PresentationRequestKey` | Signs Presentation Request objects (OID4VP) |
| `KeyEncryptionKey` | Encrypts Data Encryption Keys (DEKs) used for database encryption |
| `AdminSigningKey` | Signs and verifies access tokens for Wallet Dashboard access |

Two KMS adapters are supported:

**Local KMS adapter:** Keys are generated using Cryppo and stored in the Wallet database. Private key material is itself encrypted using a Master Encryption Key (MEK) that the operator configures in the static config file. This adapter is suitable for development and single-server environments. Without a configured MEK, keys are stored unencrypted: this is not recommended for production.

**AWS KMS adapter:** Keys are stored in and used directly via AWS KMS. Cryptographic operations (signing, key generation) are performed via the AWS KMS API, so private key material never leaves AWS KMS. Recommended for production deployments.

Data at rest is encrypted using envelope encryption: each record is encrypted with a unique Data Encryption Key (DEK). The DEK is encrypted by the Key Encryption Key and stored alongside the ciphertext. The plaintext DEK exists only in memory during the write operation.

## Configuration

Wallet configuration is split into two layers:

**Static configuration** is a minimal file the deployment owner manages. It covers what the service genuinely needs before it can start: the application host, database and Redis connections, logging level, module enablement, and the MEK for the local KMS adapter. Cryptographic keys are not part of this file; they are generated and managed by the KMS.

**Runtime settings** cover all mutable business and application settings: display metadata, supported credential formats, issuance and verification policies, trust configuration, and SVX Verify settings. These are stored in the Wallet database and can be updated at any time via the Wallet API (`/system/settings/{namespace}`) or through the configuration screens in the Wallet Dashboard, without requiring a redeployment.

Settings are organised into five namespaces: `system`, `issuer`, `verifier`, `bridge`, and `holder`. Every setting is validated against a published JSON Schema before being accepted. The merged effective configuration (static file + runtime settings + schema defaults) can always be inspected via `GET /system/settings/effective`, with secrets redacted.

See the [Configuration guide](../../guides/api-guides/wallet/configuration.md) for details.

## Wallet Dashboard

The Wallet Dashboard is a browser-based management interface served directly by the Wallet service at `/dashboard`. It is intended for wallet operators and replaces the credential and verification management screens that were previously hosted in the SVX Portal.

From the Dashboard, operators can manage:

- Credential schemas, templates, and issued credentials
- Verification templates and presentation requests and responses
- SVX Verify configuration and sessions
- API keys for application access
- Admin accounts and passkey authentication
- Wallet runtime configuration
- X.509 certificates and image assets

Admins authenticate to the Dashboard using passkeys. An external identity provider can also be configured as an alternative.

## Application access

Applications (services or scripts) that need to call the Wallet API authenticate using an API key obtained via the Application API Key flow:

1. Create an application record via `POST /application/applications` to receive a `client_id` and `client_secret`.
2. Exchange the credentials for a bearer token via `POST /application/token`.
3. Include the bearer token in subsequent API requests.

See [Using the Wallet API](../../guides/api-guides/wallet/using-the-api.md) for details.

## Supported standards

The Wallet Service implements the following key open standards:

| Capability | Standard |
|---|---|
| Credential issuance | OpenID for Verifiable Credential Issuance (OID4VCI) |
| Credential presentation | OpenID for Verifiable Presentations (OID4VP) |
| Credential format | SD-JWT VC (`dc+sd-jwt`), JWT VC JSON (`jwt_vc_json`), ISO mDOC (`mso_mdoc`) |
| Decentralised identifiers | `did:key` (including EBSI variant), `did:jwk` |
| Credential status | Verifiable Credential Status List 2021 |
| Client authentication | OAuth 2.0 (client credentials flow) |
| Admin authentication | WebAuthn / passkeys |
