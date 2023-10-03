# Issue Credentials

Issuing a credential is the first operation in the [Verifiable Credential (VC)](https://www.w3.org/TR/vc-data-model/#lifecycle-details) workflow. It enables the Issuer to create a credential so it can be delivered to the subject of the credential.

The supported options when creating a credential are:
* Generating a credential – Generates an (unsigned) credential that requires signing.
* Issuing a credential – A credential is generated and signed with keys managed on the platform.

The outcome of both of these options is a JSON formatted VC with a system-generated unique ID and an issuance date.

## Prerequisites

The following items are required in order for a credential to be created and issued:

* [Credential Schema](credential-schemas.md) – the [data schema](https://www.w3.org/TR/vc-data-model/#data-schemas) of the credential
* [Credential Type](credential-types.md)
* [DID](../dids/did-methods.md)

## Who can undertake this operation?

Any Organisation that has been onboarded to a Tenancy (by the associated Tenant), and has been assigned the role of an Issuer.

## Generate a credential

Generating a credential requires issuance data and data related to the credential subject. This data is structured following the data model of [W3C Verifiable Credential Core Data Model](https://www.w3.org/TR/vc-data-model/#core-data-model) in combination with the structure of the chosen credential schema. The following endpoint resolves the associated DIDs and performs a number of coherency checks. The result is a JWT ready to be signed.

**Endpoint**

```bash
POST /credentials/generate
```

**Request**

The request contains the following data:
* [Credential Type](credential-types.md)
* Issuer
  * DID – Fully qualified DID string
  * Name – Name of the issuer (optional)
* Claims – Maps to the `credentialSubject` attribute of a credential
  * Subject DID – Typically, the `id` property contains the DID of the subject
* Expiration date – Datetime after which the credential expires
* Revocable – If true, the generated credential can be revoked later on.

**Response**

The response is the credential object that is generated. This contains:
* ID of the credential
* Unsigned credential in `vc-jwt` data format
* Metadata
