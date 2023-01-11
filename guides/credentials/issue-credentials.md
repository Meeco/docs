# Issue Credentials

Issuing a credential is the first operation in the [Verifiable Credential (VC)](https://www.w3.org/TR/vc-data-model/#lifecycle-details) workflow. It enables the Issuer to create a credential so it can be delivered to the subject of the credential.

The supported options when creating a credential are:
* Generating a credential – generates an (unsigned) credential that requires signing.
* Issuing a credential – a credential is generated and signed with keys managed on the platform.

The outcome of both of these options is a JSON formatted VC with a system-generated unique ID and an issuance date.

## Prerequisites

The following items are required in order for a credential to be created and issued:

* [Credential Schema](../credential-schemas.md) – the [data schema](https://www.w3.org/TR/vc-data-model/#data-schemas) of the credential
* [Credential Type](../credential-types.md)
* [DID](../dids/did-methods.md)

## Who can undertake this operation?

Any Organisation that has been onboarded to a Tenancy (by the associated Tenant), and has been assigned the role of an Issuer.

## Generate a credential

Generating a credential requires issuance data and data related to the credential subject. This data is structured following the datamodel of [W3C Verifiable Credential Core Data Model](https://www.w3.org/TR/vc-data-model/#core-data-model) in combination with the structure of the chosen credential schema. The following endpoint resolves the associated DIDs and performs a number of coherency checks. The result is a JWT ready to be signed.

**Endpoint**

```bash
POST /credentials/generate
```

**Request**

The request contains the following data:
* [Credential Type](credential-types.md)
* Issuer
  * DID – fully qualified DID string
  * Name – Name of the issuer (optional)
* Claims – maps to the `credentialSubject` attribute of a credential
  * Subject DID – typically, the `id` property contains the DID of the subject
* Expiration date – datetime after which the credential expires
* Revocable – if true, the generated credential can be revoked later on.

**Response**

The response is the credential object that is generated. This contains:
* ID of the credential
* Unsigned credential in `vc-jwt` data format
* Metadata

## Issue a Credential

Issuing a credential involves [Generating](#generate-credential) a credential with the addition of the credential being signed by the given issuer. The result is a signed VC-JWT.

To sign the credential, a fully qualified DID string or object with an `id` property needs to be provided. The DID and its verification keypair needs to be under control of the platform to be able to perform the signing.

**Endpoint**

```bash
POST /credentials/issue
```

**Request**

The request contains the following data:
* [Credential Type](credential-types.md)
* Issuer
  * DID – fully qualified DID string
  * Name – Name of the issuer (optional)
* Claims
  * Subject DID – typically, the `id` property contains the DID of the subject
* Expiration date – datetime after which the credential expires
* Revocable – if true, the generated credential can be revoked later on.

**Response**

The response provides the generated credential object. This contains:
* ID of the credential
* Signed Credential in `vc-jwt` data format
* Meta data
