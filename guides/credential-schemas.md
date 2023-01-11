# Credential Schemas

Credential schemas are used to define the structure of the claims of a verifiable credential (VC) and are associated with the [Data Schema](https://www.w3.org/TR/vc-data-model/#data-schemas) property of a credential. Schemas use the [Verifiable Credentials JSON Schema 2022](https://w3c-ccg.github.io/vc-json-schemas/) specification, published by the W3C Credentials Working Group.

Using schemas in the credential workflow allows Tenants, Organisations and users to:
- Agree on the structure of data and facilitate data exchange
- Extract information from the schema

It is possible to create a credential without specifying a schema via the API, however, when using Meeco's Enterprise Portal to create credentials, this step is mandatory. In the Enterprise Portal, a user is required to upload a schema file which is then visually displayed as a form.

A credential schema, once published, is versioned and made available via a public URL. This allows anyone to validate it at any point in time.

## Prerequisites

- Verifiable Credential JSON Schema

### Verifiable Credential JSON Schema

Each JSON schema consists of the following mandatory attributes:
- type (e.g. JsonSchemaValidator2018) - The specific type definition determines the content of each data schema.
- id - A URI that identifies the schema file.

JSON schemas uploaded via the Enterprise Portal:
- Must be a plain JSON object.
- Will have the structure checked based on the specification: JSON Schema

Below is an example JSON schema. Note that all the attributes contained within a JSON schema will be used to form a credential. Attributes can be customised, and those that appear in the example below are indicative of possible options.

```bash
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "name": "Example",
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

**Request**

* Name – Name of the credential schema
* JSON Schema - JSON schema file
* List of organisations - Organisations where this credential schema can be used (optional)

**Response**

The response received is a credential schema object. Upon creation, version `1.0` is assigned.

## Read Credential Schemas

Retrieve a list of credential schemas.

**Endpoint**

```bash
GET /schemas
 ```

**Request**

* Organisation (header)

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

* The ID of the credential schema
* Name – Name of the credential schema
* List of organisations - Organisations where this credential schema can be used (optional)

**Response**

The updated credential schema object.

## Read Verifiable Credential JSON Schema

A public endpoint that returns the JSON schema file. No authentication necessary.

**Endpoint**

```bash
GET /schemas/{id}/{version}/schema.json
```

**Request**

* Id – ID of the Credential Schema
* Version – Version of the credential schema

**Response**

Returns JSON schema for a credential schema.
