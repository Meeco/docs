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

## Who can undertake this operation?

Any Organisation that has been onboarded to a Tenancy (by the associated Tenant), and has been assigned the role of an Issuer.

## Generate a credential

Generating a credential requires issuance data and data related to the credential subject. This data is structured following the data model of [W3C Verifiable Credential Core Data Model](https://www.w3.org/TR/vc-data-model/#core-data-model) in combination with the structure of the chosen credential schema. The following endpoint resolves the associated DIDs and performs a number of coherency checks. The result is a JWT ready to be signed.

**Endpoint**

```bash
POST /credentials/generate
```

**Request body**
```bash
{
  "credential": {
  "credential_type_id": {credential_type_id},
  "issuer": {
  "id": {issuer_id},
  "name": {issuer_name}
  },
  "claims": {
    {claim_1}: {claim1_value} 
  },
  "disclosure_frame": {disclosure_frame}
  "cnf": {cnf},
  "revocable": {revokable},
  "expires_at": {expires_at},
  "type": {type}
}
```

The request contains the following data:
* `credential`
  * [`credential_type_id`](credential-types.md)
  * `issuer`
    * `id` – Issuer ID
    * `name` – Name of the issuer (optional)
* `claims` – Claims for the subject
* `disclosure_frame` - Disclosure frame for vc+sd-jwt format credential (optional)
* `cnf` - VC JWKs configuration. 
* `revokable` – If true, the generated credential can be revoked later on.
* `expires_at` – Datetime after which the credential expires. Default: false


**Response**

The response is the credential object that is generated. This contains:
* ID of the credential
* Unsigned credential
* Metadata
