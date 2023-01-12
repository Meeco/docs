# Presentation Definitions

Presentation definitions define which credential(s) a Verifier requests and for what purpose. Each selected credential is comprised of a [credential schema](https://docs.meeco.me/guides/credential-schemas) and the associated Issuer. The resulting object is conformant with the [W3C Presentation Exchange 1.0](https://identity.foundation/presentation-exchange/spec/v1.0.0/) specification and is used when generating a [Verification Request](oidc4vp.md).

## Prerequisites

* [Verifiable Credential JSON Schema](credential-schemas.md)
* [Issuer DID](dids/did-methods.md) (optional)

## Who can undertake this operation?

A presentation definiton is created by an Organisation.

## Create Presentation Definition

Creation of a presentation definitions for an Organisation.

**Endpoint**

```bash
POST /presentation_definitions
```
**Request**

* Organisation (header)
* Name
* Purpose
* List of required credentials. For each, the following is defined:
  * Name
  * Purpose
  * Verifiable Credential JSON Schema URL
  * Issuer DID

**Response**

A presentation definition object is created. This presentation definition is associated with the pre-defined credential schema and the Organisation that initiated its creation.

## Read Presentation Definitions

### List

Retrieve a list of presentation definitions owned by an Organisation.

**Endpoint**

```bash
GET /presentation_definitions
```
**Request**

* Organisation (header)
* Filters (optional):
  * Status

**Response**

List of presentation definitons managed by this Organisation.

### One Object

Retrieve a presentation definition by ID. The resulting object needs to be owned by the Organisation that is making the request.

**Endpoint**

```bash
POST /presentation_definitions/{id}
```
**Request**

* Presentation Definition ID
* Organisation (header)

**Response**

A presentation definition object.

## Archive Presentation Definition

A presentation definition can be archived and restored. When a presentation definition has been archived, it cannot be used in the verification request workflow.

**Endpoint**

```bash
PUT /presentation_definitions/{id}
```
**Request**

* Presentation Definition ID
* Organisation (header)

**Response**

The updated presentation definition status will be returned, either:
* `is_archived: false` - For active or restored presentation definitions
* `is_archived: true` - For archived presentation definitions

## Read Presentation Definition JSON

Public endpoint that returns the JSON representation of a presentation definition, following the [W3C Presentation Exchange 1.0](https://identity.foundation/presentation-exchange/spec/v1.0.0/) specification.

**Endpoint**

```bash
GET /presentation_definitions/{id}/definition.json
```
**Request**

* Presentation Definition ID
* Organisation (header)

**Response**

Returns a JSON schema for a presentation definition.
