# OpenID Connect for Verifiable Presentation

[OpenID Connect (OIDC)](https://openid.net/connect/) is an open authentication protocol which is used for authentication Wallet Holders accros Wallet without having to store and manage passwords file. 
OpenID for Verifiable Presentation allows to request and deliver Verifiable Presentations un combination with independant OpenID providers and traditional OpenID providers.
## Prerequisites

- [DID](dids/did-methods.md)
- [Presentation](../presentations.md)

## Who can use this?

Used by holder and verifier in a verification flow using the OpenID Connect protocol.

## Create Presentation Request

Register Presentation Request for organisation

**Endpoint**

```bash
POST /oidc/presentations/requests
```

**Request**

* Organisation (header)

**Responses**

The presentation request object that includes an unsigned JWT. The client calling this endpoint (e.g. holder wallet) is responsible for adding the signature.

## Update Presentation Request

Update existed presentation request object for thу particular organisation.

**Endpoint**

```bash
PUT /oidc/presentations/requests/{id}
```

**Request**

* Request ID
* Organisation (header)

**Responses**

The presentation request object.

## Read Presentation Request JWT

Public endpoint that returns the (signed) presentation request JWT.

**Endpoint**

```bash
GET /oidc/presentations/requests/{id}/jwt
```

**Request**

* Request ID

**Responses**

Signed presentation request JWT token.

## Verify Presentation Request

Verification of SIOP token signature and extract the verifiable presentation, than verification of verifiable presentation structure, signatures and if data provided match presentation definition.

**Endpoint**

```bash
POST /oidc/presentations/requests/verify
```

**Request**

* Signed presentation request JWT

**Responses**

The result of the verification, either true or false.

## Create Presentation Response

Generate id_token for request submission based on the Wallet information and the verifiable presentation token

**Endpoint**

```bash
POST /oidc/presentations/token
```

**Request**

* Presentation Request JWT

**Responses**

The presentation response object that includes two unsigned JWT, `id_token` and `vp_token`. The client calling this endpoint (e.g. holder wallet) is responsible for adding the signatures for each token.

## Verify Presentation Response

Verify the presentation response to a given request. The steps performed are:

- Verify ID Token
- Verify VP Token
  - [Verify presentation](../presentations.md)
- Verify if the response is valid for the given request

**Endpoint**

```bash
POST /oidc/presentations/response/verify
```

**Request**

* Presentation Request JWT
* Signed ID Token
* Signed VP Token

**Responses**

The result of the verification, either true or false. In case of false, all errors are provided, with an explanation.