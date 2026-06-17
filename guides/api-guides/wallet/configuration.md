# Configuration

SVX Wallet configuration is split into two clearly separated layers under a single contract:

* **Static configuration** – a minimal file the deployment owner manages, reduced to what the service genuinely needs before it can start.
* **Runtime settings** – all mutable business and application settings, stored in the database and managed through the Wallet API or the SVX Wallet Dashboard.

Both layers are governed by JSON Schemas that ship with the service (`config/config.schema.json` and `config/runtime.schema.json`). The schemas define every available option, its constraints, and its default value, and are the authoritative reference for both layers. The static file is validated when the service starts, and runtime documents are validated on every write.

The result is that deployment configuration stays small and stable, day-to-day settings changes need no redeployment, and the effective deployed configuration is always observable with secrets redacted.

## Static configuration

The static configuration is a single JSON file (`config/config.json`) that covers what must be known before the service can start: the application host, database and Redis connections, logging, module enablement, KMS settings, and secret material supplied by the operator.

Cryptographic keys are not part of the configuration. They are generated and managed by the configured Key Management Service (KMS); the file carries only the material the operator must supply, such as database credentials, KMS access credentials and integration secrets.

The file contains five top-level blocks:

| Block | Contents |
| --- | --- |
| `system` | Application host, PostgreSQL and Redis connections, logging, KMS, monitoring, image upload limits, and the Dashboard. |
| `holder` | Client authentication and attestation settings for holder wallet flows. |
| `bridge` | Bridge module enablement, wallet setup defaults, and integration secret material. |
| `issuer` | Issuer module enablement, the public issuer identifier, trust anchors, and the built-in authorization server bootstrap. |
| `verifier` | Verifier and SVX Verify module enablement and bootstrap settings. |

The file is validated against the configuration schema at startup, including semantic checks across related options; the service does not start with an invalid configuration. Declaring the schema in the file enables validation and autocompletion in most editors.

A minimal configuration looks like this:

```json
{
  "$schema": "./config.schema.json",
  "system": {
    "app_host": "https://wallet.example.com",
    "postgres": {
      "host": "postgres.internal",
      "database": "svx_wallet",
      "username": "svx_wallet",
      "password": "<secret>"
    },
    "redis": {
      "host": "redis.internal",
      "database_number": 0,
      "password": "<secret>",
      "tls": true
    },
    "logging": {
      "log_level": "info",
      "log_redact_properties": []
    }
  },
  "holder": {},
  "bridge": {
    "enabled": false,
    "external_reference": "my-deployment",
    "wallet_key_type": { "kty": "EC", "crv": "P-256" }
  },
  "issuer": {
    "internal_authorization_server": {
      "dpop_enabled": false,
      "supported_client_auth_methods": []
    }
  },
  "verifier": {}
}
```

### Environment variable overrides

Environment variables override database credentials only:

| Variable | Overrides |
| --- | --- |
| `POSTGRES_USER` | `system.postgres.username` |
| `POSTGRES_PASSWORD` | `system.postgres.password` |
| `POSTGRES_DB_NAME` | `system.postgres.database` |

No other configuration option can be set through the environment.

## Runtime settings

All mutable business and application settings live in the database as one document per namespace. There are five namespaces, mirroring the static blocks:

| Namespace | Contents |
| --- | --- |
| `system` | Gateway trust (JWKS URLs) and Dashboard authentication settings. |
| `issuer` | Display metadata, supported credentials, issuance policy and metadata, claims mapping, authorization server policy, and the resource hook. |
| `verifier` | Trust configuration, presentation request policy, and SVX Verify settings. |
| `holder` | Trust configuration for holder flows. |
| `bridge` | Identity provider integrations and issuer wallet links. |

Runtime settings are managed through the Wallet API (below) or the SVX Wallet Dashboard. Every write is validated against the runtime settings schema, recorded with the administrator who made it, and applied to all running instances without a redeployment — instances are notified of changes and additionally perform a periodic full reconcile.

Each namespace document carries a revision number used for optimistic concurrency: updates must state the revision they were based on, and a write is rejected if the document has changed in the meantime.

### Read settings

**Endpoint**

```bash
GET /system/settings            # all namespaces
GET /system/settings/{namespace}
```

**Response**

```json
{
  "setting": {
    "namespace": "issuer",
    "configured": true,
    "revision": 4,
    "document": {
      "display": { "name": "Example Organisation" }
    },
    "updated_at": "2026-06-01T09:30:00.000Z",
    "updated_by": "0d4f7c6e-4a9b-4f6e-9a52-1f2ab33c9d10"
  }
}
```

A namespace that has never been written returns `configured: false` with a `null` revision, and the service runs on defaults for that namespace.

### Update a namespace

**Endpoint**

```bash
PUT /system/settings/{namespace}
```

**Request**

```json
{
  "document": {
    "display": { "name": "Example Organisation" }
  },
  "expected_revision": 4
}
```

* `document` – the full settings document for the namespace, validated against the runtime settings schema
* `expected_revision` – the revision this update was based on; use `0` for the first write to a namespace

The response returns the stored document with its new revision. If `expected_revision` does not match the current revision, the write is rejected and the document is left unchanged.

### Reset a namespace

**Endpoint**

```bash
DELETE /system/settings/{namespace}
```

Resets the namespace to its defaults. The request body carries the `expected_revision`, following the same concurrency rule as updates.

## Effective configuration

At runtime, the service merges the runtime settings over the static configuration and the schema defaults to form the *effective configuration* — the configuration the service is actually running with. It can be inspected at any time:

**Endpoint**

```bash
GET /system/settings/effective
```

**Response**

```json
{
  "values": {
    "system": {
      "app_host": "https://wallet.example.com",
      "postgres": {
        "host": "postgres.internal",
        "password": "***REDACTED***"
      }
    },
    "issuer": {
      "display": { "name": "Example Organisation" }
    }
  }
}
```

> **Note**
> Secret material — passwords, private keys, cookie secrets and similar — is always redacted in the effective configuration. It is never returned by the API.
