# Issue Credentials

Issuing a credential is a core operation in the Verifiable Credential (VC) workflow. In SVX Wallet API there are two supported issuance approaches:

* Ad-hoc issuance - issue credentials directly through SVX Wallet API endpoints.
* [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) - issue credentials through the interoperable OID4VCI protocol flow.

Both approaches result in a verifiable credential payload that can be delivered to the subject's wallet.

Issued credentials can be reviewed in the SVX Wallet Dashboard. Revocation via the dashboard is currently supported for `jwt_vc_json` credentials, and support for other credential formats is planned in the near future.

## Prerequisites

The following items are required in order for a credential to be created and issued:

* [Credential Templates](credential-templates.md)
* Subject's public JWK or DID reference in most cases managed by the holder wallet client applications.

## Who can undertake this operation?

Who can run issuance depends on the selected approach:

* Ad-hoc issuance: It can only be run by people or applications that have access to the SVX Wallet Dashboard and API.
* OID4VCI issuance: OID4VCI Wallet Clients (acting for an End-User) as defined by the [OID4VCI specification](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html), interacting with the Credential Issuer and Authorization Server roles.

## Ad-hoc issuance

Ad-hoc issuance creates a credential directly through the SVX Wallet API using a credential template and claim values.

**Endpoint**

```bash
POST /issuer/credentials
```

Use the following example to issue a credential with the Admin API:

```bash
curl -X POST "$SVX_WALLET_BASE_URL/issuer/credentials" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "credential_template_id": "<credential_template_id>",
    "claims": {
      "email": "alice@example.com",
      "is_over_18": true
    },
    "binding_keys": [
      {
        "jwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "<subject_public_jwk_x>",
          "y": "<subject_public_jwk_y>"
        }
      }
    ]
  }'
```

The API returns the issued credential object and related metadata.

## OID4VCI issuance

OID4VCI issuance follows the OpenID for Verifiable Credential Issuance protocol and supports wallet-initiated and issuer-initiated flows.

Important: the credential offer endpoint set can be used to start an issuer-initiated flow.

The API supports both OID4VCI grant flows:

- Authorization Code flow
- Pre-Authorized Code flow

`issuer_state` and `pre-authorized_code` are optional in credential offers. If either is omitted, the API generates a random value.

The main endpoints involved are:


Credential Issuer endpoints:

- `POST /issuer/offers` - Create a credential offer (issuer-initiated flow start).
- `GET /issuer/offers/{id}` - Read credential offer details used by the wallet.
- `DELETE /issuer/offers/{id}` - Delete a credential offer.
- `GET /.well-known/openid-credential-issuer` - Credential Issuer metadata.
- `POST /issuer/nonce` - Nonce endpoint.
- `POST /issuer/credential` - Credential endpoint.

Authorization Server endpoints:

- `GET /.well-known/oauth-authorization-server` - Authorization Server metadata.
- `GET /authorize` - Authorization endpoint.
- `POST /par` - Pushed Authorization Request endpoint (when required).
- `POST /token` - Token endpoint.

Use the following example to create a credential offer for issuer-initiated OID4VCI flow using Pre-Authorized Code:

```bash
curl -X POST "$SVX_WALLET_BASE_URL/issuer/offers" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "credential_template_id": "<credential_template_id>",
    "claims": {
      "email": "alice@example.com",
      "is_over_18": true
    },
    "grants": {
      "urn:ietf:params:oauth:grant-type:pre-authorized_code": {}
    }
  }'
```

After the offer is created, the wallet can be given an `openid-credential-offer://` URL that references the created offer:

```bash
openid-credential-offer://?credential_offer_uri=https%3A%2F%2F%3Csvx_wallet_base_url%3E%2Fissuer%2Foffers%2F%3Coffer_id%3E
```

For wallet-initiated OID4VCI flow, a wallet client can open an Authorization Endpoint URL with a credential scope value from issuer metadata:

```bash
https://<svx_wallet_base_url>/authorize?response_type=code&client_id=<wallet_client_id>&redirect_uri=<wallet_redirect_uri_encoded>&scope=<credential_scope_from_issuer_metadata>
```
