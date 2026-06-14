# SVX 4.0.0 Release Notes

**Software Release Date:** 22 June 2026 (date is indicative)

## Summary

SVX 4.0 is a major architectural release representing the most significant restructuring of the SVX platform since its inception in April 2023. This release consolidates the platform from more than ten (micro)services into four core services, reducing operational complexity. The key outcomes are faster development velocity, a smaller attack surface for personally identifiable information (PII), simplified deployments, and a unified wallet service that can serve as issuer, verifier, and wallet provider simultaneously.

- Unified, standalone Wallet Service
- PII never leaves Wallet service; encrypted storage with envelope encryption (AES-256-GCM).
- Cost and reliability: fewer runtimes, no RabbitMQ (DB-driven queues), simpler deployments.

Besides the new architecture we bring the following features to you faster. 
- Wallet KMS Integration
- Wallet Simplified Configuration

## Breaking Changes

> **Important:** SVX 4.0 introduces breaking changes across the API surface. Please review the relevant sections carefully before upgrading.

- The Organisation Wallet (OW) is merged into the new unified Wallet service.
- The Holder Wallet (HW) is merged into the new unified Wallet service.
- Restructuring of OW and HW API endpoints in the new unified Wallet service.
- Vault and Keystore APIs are removed. Encrypted credential storage and key management are now handled directly within the Wallet service.
- The VC service API is removed. Schema management, credential type management, presentation definition management, and credential operations are now available through the Wallet API.
- The authorize endpoint for bridge installations is split off and hosted at `/bridge/authorize`. The callback endpoint `/authorize/receive/callback` becomes `/bridge/authorize/receive/callback`.
- ISO Mobile Document credential storage format has changed. Credentials are now stored as `IssuerSigned` rather than `DeviceResponse`. There is no automatic migration of previously stored credentials.
- Wallet configuration is no longer exclusively file-based. Most configuration is now managed at runtime, persisted in the application database, and accessible via the Wallet API or the Wallet Admin UI.

## New Features

### Unified Wallet Service

The most significant change in SVX 4.0 is the introduction of a single, unified Wallet service that combines the roles of issuer, verifier, wallet provider, and credential bridge. Previously these roles were spread across separate deployments with separate configurations. Now a single deployment can serve all roles simultaneously.

This means less infrastructure to manage, fewer points of failure, and a single API surface to integrate with. For customers building on SVX, this dramatically reduces the complexity of getting a working credential issuance and verification flow running.

### PII Containment

In previous versions of SVX, issuing a verifiable credential required PII to pass through multiple services: the Organisation Wallet, the VC API, and the Vault (for storage). Each additional service that touches PII increases the attack surface and complicates compliance.

In SVX 4.0, PII never leaves the Wallet service boundary. Credential data is encrypted at rest within the Wallet. This makes it substantially easier to reason about where sensitive data lives and to demonstrate compliance with data protection requirements.

### Simplified Credential Issuance and Verification APIs

Credential management, schema management, and verification template management are now consolidated into the Wallet API. The new endpoints follow a consistent, role-prefixed structure under

- `/issuer/*`
- `/verifier/*`
- `/wallets/*`
- `/verify/*`
- `/bridge/*`

Endpoints related to standards specifications, such as OAuth 2.0 and OpenID4VCI, are not prefixed.

Below is an overview of the new endpoints:

#### Schemas and templates

```
POST   /issuer/schemas
GET    /issuer/schemas
GET    /issuer/schemas/{id}
PUT    /issuer/schemas/{id}
GET    /issuer/schemas/{id}/schema.json
DELETE /issuer/schemas/{id}

POST   /issuer/templates
GET    /issuer/templates
GET    /issuer/templates/{id}
PUT    /issuer/templates/{id}
DELETE /issuer/templates/{id}

POST   /verifier/templates
GET    /verifier/templates
GET    /verifier/templates/{id}
PUT    /verifier/templates/{id}
DELETE /verifier/templates/{id}
```

Schemas and templates no longer use archiving, deleting them removes it from the database. Each entity can be updated directly. There is no versioning of schemas anymore.

#### Credential offers
```
POST   /issuer/offers
GET    /issuer/offers/{id}
DELETE /issuer/offers/{id}
```

#### Issued credentials
```
POST   /issuer/credentials
GET    /issuer/credentials
GET    /issuer/credentials/{id}
DELETE /issuer/credentials/{id}
PATCH  /issuer/credentials/{id}/status
```

#### Credential status lists
```
GET /issuer/status_list/{id}
```

#### Presentation requests
```
POST   /verifier/requests
GET    /verifier/requests
GET    /verifier/requests/{id}
DELETE /verifier/requests/{id}
GET    /verifier/requests/{id}/responses
GET    /verifier/requests/{id}/responses/{id}
DELETE /verifier/requests/{id}/responses/{id}
```

### SVX Verify

The `identity` prefix has been renamed to `verify` across all SVX Verify endpoints.
```
POST /verify/sessions
GET  /verify/sessions
GET  /verify/sessions/{id}
GET  /verify/verification/{id}/request
GET  /verify/verification/{id}/status
POST /verify/verification/{id}/accept_terms
```

`GET /verify/sessions` is a new endpoint to facilitate session listing.

### Bridge

What used to be a combination of two components configured in a very specific way has now merged into one. The bridge module handles account based flows using OpenID Connect and creates derived credentials which can be consumed by verifiers using OID4VP. 

```
GET /bridge/authorize
GET /bridge/authorize/receive/callback
```

Bridge functionality can be used via SVX Verify with selected integrations.

### OpenID4VCI
```
POST /issuer/credential
POST /issuer/nonce
```

### OAuth Authorization Server
```
GET  /authorize
POST /par
POST /token
POST /challenge
GET  /jwks
```

### OpenID4VP
```
GET  /verifier/requests/{id}/jwt
POST /verifier/requests/{id}/responses
```

### Well-Known Endpoint
```
GET /.well-known/oauth-authorization-server
GET /.well-known/openid-configuration
GET /.well-known/openid-credential-issuer
GET /.well-known/jwt-vc-issuer
GET /.well-known/appspecific/selectid.rp
```

### Wallet Issuer/Verifier Identifier

The Wallet supports the following issuer identifier types: URL, DID, and X.509. Supported DID methods are `did:key` (including the EBSI variant) and `did:jwk`.

### Wallet x509 Certificate Management
```
GET    /system/certificates
POST   /system/certificates/import
DELETE /system/certificates/{certificate_id}
GET    /system/certificates/csrs
POST   /system/certificates/csrs
DELETE /system/certificates/csrs/{csr_id}
POST   /system/certificates/csrs/{csr_id}/import_certificate
```

### Application API Key
```
POST   /application/applications
GET    /application/applications
DELETE /application/applications/{id}
POST   /application/token
```

### Admin Login via Passkeys Endpoint
```
GET    /admin/accounts
DELETE /admin/accounts/{id}
POST   /admin/invitations
GET    /admin/invitations
DELETE /admin/invitations/{id}
POST   /admin/invitations/accept/options
POST   /admin/invitations/accept
POST   /admin/sessions/options
POST   /admin/sessions
POST   /admin/sessions/refresh
```

### Image assets upload
```
POST /assets/images
GET  /assets/images/{id}
```

Supported formats are PNG, JPEG, and WebP. Uploads require admin authentication. Retrieval is unauthenticated so that stored images can be served directly to wallets and verifier UIs.

### Wallet Dashboard

A new browser-based Wallet Dashboard is now served directly by the Wallet service. It replaces the credential and verification management screens previously hosted in the Portal.

Functionality available in the Wallet Dashboard:

- Credential Schemas
- Credential Templates
- Verification Templates
- Issued Credentials
- Presentation Requests and Submissions
- Verify sessions and reporting
- Manage API Keys
- Wallet configuration

### Wallet KMS Integration

Cryptographic Keys required for base functionalities no longer need to be provided via configuration. Keys will be automatically generated and stored by the Wallet KMS.

#### Key adapters

- **Local KMS Adapter** — keys are stored in the attached database service. Suitable for development and environments without an external KMS.
- **AWS KMS Adapter** — keys are stored in AWS KMS. Cryptographic operations such as signing are done via API calls to AWS KMS.

#### Key roles

- Credential Issuance
- Credential signing
- Access token signing
- Presentation Verification
- Presentation Request object signing
- Database Encryption
- Key Encryption Key (KEK)
- Utility Keys
- Admin Dashboard access token signing

### Wallet Encrypted Storage

Data is encrypted at rest using envelope encryption using Meeco's Cryppo library. Each record is encrypted with a unique Data Encryption Key (DEK). The default encryption algorithm is AES-256-GCM.

#### KMS Key providers

- **Local KMS provider** — the KEK is generated using Cryppo and stored in the database.
- **AWS KMS provider** — uses GenerateDataKey to atomically create and wrap the DEK in a single KMS API call.

### New Wallet Configuration Approach

SVX 4.0 restructures configuration into two clearly separated layers:

- **Static configuration:** a minimal file reduced to what the service genuinely needs before it can start.
- **Runtime settings:** all mutable business and application settings live in the database and are managed via the Wallet API or Dashboard.

#### Application Runtime Configuration

Mutable application settings are organised into five namespaces: system, issuer, verifier, bridge, and holder.
```
GET    /system/settings
GET    /system/settings/effective
GET    /system/settings/{namespace}
PUT    /system/settings/{namespace}
DELETE /system/settings/{namespace}
```

#### Effective Configuration

The effective configuration can be inspected at any time via `GET /system/settings/effective`.

## Changes and Removals

### Portal Changes

The Portal is now scoped to platform and scheme administration. Credential and verification management screens have moved to the new Wallet Dashboard.

**Moved to Wallet Dashboard:**

- Credential Schemas (including all sub-pages)
- Credential Templates (including all sub-pages)
- Verification Templates (including all sub-pages)
- Credentials Issued (including all sub-pages)
- Presentation Requests and Submissions (including all sub-pages)

**Removed:**

- Vault Connections
- Tenant Applications
- Organisation Keys and Passphrase management screens
- Tenant End Users

**Kept:**

- All Global Admin screens
- All Tenant Admin screens
- Organisation: Applications and User Management

### ISO Mobile Document Storage

Previously, issued ISO Mobile Document credentials were stored as `DeviceResponse`, this is changed to `IssuerSigned`.

## Migration Notes

### Credential Data Stored in SVX API

There is no automated migration of credential data currently stored in the SVX API to the new Wallet. New deployments should start with a clean state. This applies to:

- Credential schemas
- Credential templates
- Presentation templates
- Credentials issued (issuer)
- Presentation responses received (verifier)
- Credentials received (wallet)

### Existing Wallet Instances

Because of the structural changes to the SVX platform, older Organisation and Holder Wallet services will not be compatible with the new SVX 4.0 release.
