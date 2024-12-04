# Credential Schemas

Credential schemas are used to define the structure of the claims of a verifiable credential (VC) and are associated with the [Data Schema](https://www.w3.org/TR/vc-data-model/#data-schemas) property of a credential. Schemas use the [Verifiable Credentials JSON Schema](https://www.w3.org/TR/vc-json-schema/) specification, published by the W3C Credentials Working Group.

Using schemas in the credential workflow allows Tenants, Organisations and users to:
- Agree on the structure of data and facilitate data exchange
- Extract information from the schema

It is possible to create a credential without specifying a schema via the API, however, when using Meeco's Enterprise Portal to create credentials, this step is mandatory. In the Enterprise Portal, a user is required to upload a schema file which is then visually displayed as a form.

A credential schema, once published, is versioned and made available via a public URL. This allows anyone to validate it at any point in time.

## Prerequisites

- Verifiable Credential JSON Schema

### Verifiable Credential JSON Schema

Each VC JSON Schema consists of the following mandatory attributes:
- Name - A locally unique identifier to address the VC schema.
- JSON Schema – Object that describes the schema the credential is validated against.

[JSON schemas](https://json-schema.org/) are plain JSON objects that cosist of following mandatory attributes (at the top level):
- `$schema` - The schema specification used (e.g. `https://json-schema.org/draft/2020-12/schema`)
- `required` – Array of required properties.
- `type` – MUST be `object`
- `additionalProperties` - MUST be `true`

Below is an example JSON schema. Note that all the attributes contained within a JSON schema will be used to form a credential. Attributes can be customised, and those that appear in the example below are indicative of possible options.

```bash
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Example title",
  "description": "Example description",
  "type": "object",
  "properties": {
    "student_id": {
      "type": "string",
    }
  },
  "required": ["student_id"],
  "additionalProperties": true
}
```

Note that the `$id` property of the JSON Schema is set by the platform. When one is present in the schema, it will be overridden.  
Note that if the format is `jwt_vc_json`, you need to define `credentialSubject` and store claims under it.  
Here's the example of credential schema for `jwt_vc_json`.

```bash
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Example title",
  "description": "Example description",
  "type": "object",
  "properties": {
    "credentialSubject": {
      "type": "object",
      "properties": {
        "student_id": {
        "type": "string",
        }
      },
      "required": [
        "student_id"
      ],
      "additionalProperties": true
    },
  },
  "required": [
    "credentialSubject"
  ]
}
```

## Who can undertake this operation?

Credential schemas are created and managed by Tenant Administrators who can assign them to Organisations.

An Organisation can list the credential schemas that are assigned to them.

Anyone can read the JSON schema (via a separate endpoint) that is part of the credential schema object.

## Create Credential Schema

Creation of a credential schema.

**Endpoint**

```bash
POST /schemas
```

**Request body**
```bash
{ "schema":
  { "name":{name},
    "schema_json": {json_schema}
    "organization_ids" : [{organisation_id}, ...]
  }
}`

* `name` – Name of the credential schema
* `json_schema` - JSON schema file
* `organisation_id` - Organisations where this credential schema can be used (optional)

**Response**

The response received is a credential schema object. Upon creation, version `1.0` is assigned.

## Read Credential Schemas

Retrieve a list of credential schemas.

**Endpoint**

```bash
GET /schemas
 ```

**Request**
Include the following in the header section.  
* `Meeco-Organisation-ID` - An organisation ID where schemas are available to.  

**Response**

A list of credential schema objects available to the user.

In the context of an Organisation, the `organization_ids` attribute contains only one item: caller organisation ID.

## Update Credential Schema

Update an existing credential schema by ID.

Note that in this version, the schema cannot be updated.

**Endpoint:**

```bash
PUT /schemas/{id}
 ```

**Request**
* id - The ID of the credential schema

**Request body**
```bash
{ "schema":
  { "name":{name},
    "organization_ids" : [{organisation_id}, ...]
  }
}
 ```
* `name` – Name of the credential schema
* `organization_id` - List of organisation IDs. Organisations where this credential schema can be used (optional)



**Response**

The updated credential schema object.  

## Read Verifiable Credential JSON Schema

A public endpoint that returns the JSON schema file. No authentication necessary.

**Endpoint**

```bash
GET /schemas/{id}/{version}/schema.json
```

**Request**
* `id` – ID of the Credential Schema
* `version` – Version of the credential schema

**Response**

Returns JSON schema for a credential schema.
