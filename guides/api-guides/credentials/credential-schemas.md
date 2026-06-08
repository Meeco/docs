# Credential Schemas

Credential schemas are used to define the structure of the claims of a verifiable credential (VC) and are associated with the [Data Schema](https://www.w3.org/TR/vc-data-model/#data-schemas) property of a credential. Schemas use the [Verifiable Credentials JSON Schema](https://www.w3.org/TR/vc-json-schema/) specification, published by the W3C Credentials Working Group.

Using schemas in the credential workflow allows Tenants, Organisations and users to:
- Agree on the structure of data and facilitate data exchange
- Extract information from the schema

It is possible to create a credential without specifying a schema via the API, however, when using Meeco's SVX Wallet to create credentials, this step is mandatory. In the Meeco SVX Wallet, a user is required to define a credential schema which is then used to validate incoming credential claims.

A credential schema, once published is made available via a public URL. This allows anyone to validate it at any point in time.

## Prerequisites

- Verifiable Credential JSON Schema

### Verifiable Credential JSON Schema

Each VC JSON Schema consists of the following mandatory attributes:
- Name - A locally unique identifier to address the VC schema.
- Format - VC format that defined schema is expected to be used with.
- JSON Schema – Object that describes the schema the credential is validated against.

[JSON schemas](https://json-schema.org/) are plain JSON objects that cosist of following mandatory attributes (at the top level):
- `$schema` - The schema specification used (e.g. `https://json-schema.org/draft/2020-12/schema`)
- `required` – Array of required properties.
- `additionalProperties` – MUST be `false`
- `type` – MUST be `object`

Below is an example JSON schema. Note that all the attributes contained within a JSON schema will be used to form a credential. Attributes can be customised, and those that appear in the example below are indicative of possible options.

```bash
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Example",
  "description": "Example",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
    }
  },
  "required": ["id"],
  "additionalProperties": false,
}
```

Note that the `$id` property of the JSON Schema is set by the platform. When one is present in the schema, it will be overridden.

Also note that `https://json-schema.org/draft/2020-12/schema` a prefferred version but `https://json-schema.org/draft/2019-09/schema` and `https://json-schema.org/draft-07/schema` are supported as well.

### Verifiable Credential formats

The platform supports multiple credential formats. The JSON Schema structure differs depending on the format selected for a credential schema, because each format uses a different claim model and naming convention.

For `jwt_vc_json` credentials, claims are typically modelled under `credentialSubject`. The following example shows a minimal schema containing one claim (`emailAddress`) in `credentialSubject` [(VC Data Model)](https://www.w3.org/TR/vc-data-model-2.0/).

```bash
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "EmailCredential",
  "description": "EmailCredential using JsonSchema",
  "type": "object",
  "properties": {
    "credentialSubject": {
      "type": "object",
      "properties": {
        "emailAddress": {
          "type": "string",
          "format": "email"
        }
      },
      "required": ["emailAddress"],
      "additionalProperties": false
    }
  },
  "required": ["credentialSubject"],
  "additionalProperties": false
}
```

For `dc+sd-jwt` [(SD JWT VC)](https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/) credentials, claims are represented as top-level properties and include required protocol claims such as issuer (`iss`), credential type (`vct`) and confirmation (`cnf`).

```bash
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "vct": {
      "type": "string"
    },
    "iss": {
      "type": "string"
    },
    "cnf": {
      "type": "object"
    },
    "email": {
      "type": "string"
    },
    "is_over_18": {
      "type": "boolean"
    }
  },
  "required": ["iss", "vct", "cnf", "email", "is_over_18"],
  "additionalProperties": false
}
```

For `mso_mdoc` ([ISO/IEC 18013-5:2021](https://www.iso.org/standard/69084.html)) credentials, claims are grouped into namespaces (for example `org.iso.18013.5.1`). Namespace entries can include additional encoding hints when required by the data model.

The `x-cbor-encoding` extension can be used on specific schema properties to indicate how a value should be represented in CBOR.

Currently supported `x-cbor-encoding` values are:

| Value | Description | Typical JSON Schema usage |
| --- | --- | --- |
| `bstr` | Encodes the value as a CBOR byte string (major type 2). | `{ "type": "string", "x-cbor-encoding": "bstr" }` |
| `bigint` | Encodes an arbitrary precision integer using CBOR bignum representation. | `{ "type": "string", "x-cbor-encoding": "bigint" }` |
| `encoded-cbor` | Encodes embedded CBOR data as a CBOR-encoded byte string (tag 24). | `{ "type": "string", "x-cbor-encoding": "encoded-cbor" }` |

Use `x-cbor-encoding` only when a claim requires a non-default CBOR representation. If not specified, standard CBOR encoding is applied based on the JSON value type.

```bash
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "org.iso.18013.5.1": {
      "type": "object",
      "properties": {
        "email": {
          "type": "string"
        },
        "portrait": {
          "type": "string",
          "description": "jpeg as base64url encoded",
          "x-cbor-encoding": "bstr"
        }
      },
      "required": ["email", "portrait"],
      "additionalProperties": false
    }
  },
  "required": ["org.iso.18013.5.1"],
  "additionalProperties": false
}
```



## Who can undertake this operation?

Credential schemas can only be created and managed by people who have access to the SVX Wallet Dashboard and API.

Anyone can read the JSON schema (via a separate endpoint) that is part of the credential schema object.

## Create Credential Schema

Creation of a credential schema.

**Endpoint**

```bash
POST /issuer/schemas
```

Use the following example to create a schema with the Admin API:

```bash
curl -X POST "$SVX_BASE_URL/issuer/schemas" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "schema": {
      "name": "EmailCredentialSdJwt",
      "format": "dc+sd-jwt",
      "schema_json": {
        "$schema": "https://json-schema.org/draft/2020-12/schema",
        "title": "EmailCredentialSdJwt",
        "description": "Email credential schema using dc+sd-jwt",
        "type": "object",
        "properties": {
          "vct": {
            "type": "string"
          },
          "iss": {
            "type": "string"
          },
          "cnf": {
            "type": "object",
            "additionalProperties": true
          },
          "email": {
            "type": "string"
          },
          "is_over_18": {
            "type": "boolean"
          }
        },
        "required": ["iss", "vct", "cnf", "email", "is_over_18"],
        "additionalProperties": false
      }
    }
  }'
```

The API returns the created credential schema object, including its generated `id` and version metadata.

## Remaining endpoints

The following endpoints are used to maintain and read schema information:

- `GET /issuer/schemas` - List credential schemas available to the caller.
- `GET /issuer/schemas/{id}` - Read credential schema metadata by ID.
- `PUT /issuer/schemas/{id}` - Update credential schema metadata by ID.
- `DELETE /issuer/schemas/{id}` - Delete credential schema by ID.
- `GET /issuer/schemas/{id}/schema.json` - Read the public JSON schema document.
