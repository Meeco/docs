# Present Credentials

When a verifier asks a holder to prove something, the holder responds with a **verifiable presentation** — a package of one or more stored credentials, signed by the holder, that satisfies the verifier's request. The SVX Wallet handles this with the OpenID4VP flow.

Presenting a credential is a sequence of three calls, each returning a `state` value that links the steps of a single flow; pass it to the next call:

1. **Register** the verifier's authorization request.
2. **Set credentials** — choose which stored credentials answer the request, and which claims to disclose.
3. **Submit** the presentation to the verifier.

## Prerequisites

* An existing **wallet**, identified by its `walletId`. See [Manage Wallets](manage-wallets.md).
* At least one **stored credential** that satisfies the verifier's request, together with the holder key it is bound to. Credentials are added to a wallet by receiving or importing them — see [Add Credentials to a Wallet](add-credentials.md).

## 1. Register the authorization request

A presentation begins with an authorization request from a verifier, typically delivered as an `openid-vc://` URI (for example, encoded in a QR code) containing a `request_uri`. Register it with the wallet to start the flow.

**Endpoint**

```bash
POST /wallets/{walletId}/send
```

**Request**

```json
{
  "authorization_request_uri": "openid-vc://?request_uri=https://verifier.example.com/oidc/presentations/requests/dd85a4cc-cea8-4bbe-9349-00615540177e/jwt"
}
```

**Response**

The wallet resolves and verifies the request and returns the `state` along with the verifier's requirements. The `presentation_definition` describes which credentials and claims are requested; its `input_descriptors[].id` values identify each requested credential and are used in the next step.

```json
{
  "state": "3d8c088a-c23c-4038-923c-a1e9dabffc0e",
  "client_id": "https://verifier.example.com",
  "nonce": "4f0mudxzyg",
  "presentation_definition": {
    "id": "f6cee503-6da7-41cb-ac0e-1d3e796d7f10",
    "name": "Simple Identity Credential",
    "input_descriptors": [
      {
        "id": "3f87c2f0-12c8-489f-baf4-09542d107493",
        "constraints": {
          "limit_disclosure": "required",
          "fields": [
            { "path": ["$.vct"], "filter": { "type": "string", "const": "StudentID" } }
          ]
        }
      }
    ],
    "format": { "dc+sd-jwt": { "alg": ["ES256"] } }
  },
  "selected_credentials": [],
  "meta": { "submission_status": "pending" }
}
```

> **Note**
> _DCQL requests:_ a verifier may instead express its requirements using DCQL, in which case the response carries a `dcql_query` rather than a `presentation_definition`. The flow is identical; you simply match credentials against the DCQL credential query in the next step (see below).

## 2. Set the credentials to present

Choose which stored credential answers each part of the request and which claims to reveal. Find a suitable credential among the wallet's stored credentials with `GET /wallets/{walletId}/credentials` (see [Credentials](../credentials/README.md)), and use its `id` and the `kid` of the key it is bound to.

Each entry links a stored credential to a requested credential. For a Presentation Exchange request, match on `input_descriptor_id` (the `id` from the request's `input_descriptors`). For a DCQL request, match on `credential_query_id` instead.

**Endpoint**

```bash
POST /wallets/{walletId}/send/set_credentials
```

**Request**

```json
{
  "state": "3d8c088a-c23c-4038-923c-a1e9dabffc0e",
  "credentials": [
    {
      "input_descriptor_id": "3f87c2f0-12c8-489f-baf4-09542d107493",
      "credential": {
        "id": "b3f1c0e2-1d4a-4b8e-9c2a-7f6d5e4c3b2a",
        "claims_to_disclose": ["given_name"],
        "kid": "qGnIC-IM5uNB4-bmhVqerfcKBgALZknRb9cvcDNeo_0"
      }
    }
  ]
}
```

* `input_descriptor_id` – the requested credential this entry answers (use `credential_query_id` for DCQL requests)
* `credential.id` – the wallet's `id` for the stored credential to present
* `credential.kid` – the holder key the credential is bound to, used to sign the presentation
* `credential.claims_to_disclose` – the claims to reveal

> **Note**
> _Selective disclosure:_ for `dc+sd-jwt` credentials, only the claims listed in `claims_to_disclose` are revealed to the verifier; all other claims stay hidden. When the request sets `limit_disclosure` to `required`, you must disclose no more than the requested claims. Formats without selective disclosure (such as `jwt_vc_json`) are presented in full regardless of this field.

**Response**

The wallet records the selection and echoes it back; `submission_status` remains `pending` until you submit.

```json
{
  "state": "3d8c088a-c23c-4038-923c-a1e9dabffc0e",
  "credentials": [
    {
      "input_descriptor_id": "3f87c2f0-12c8-489f-baf4-09542d107493",
      "credential": {
        "id": "b3f1c0e2-1d4a-4b8e-9c2a-7f6d5e4c3b2a",
        "claims_to_disclose": ["given_name"],
        "kid": "qGnIC-IM5uNB4-bmhVqerfcKBgALZknRb9cvcDNeo_0"
      }
    }
  ],
  "meta": { "submission_status": "pending" }
}
```

## 3. Submit the presentation

Build the verifiable presentation from the selected credentials, sign it, and send it to the verifier.

**Endpoint**

```bash
POST /wallets/{walletId}/send/submit
```

**Request**

```json
{
  "state": "3d8c088a-c23c-4038-923c-a1e9dabffc0e"
}
```

**Response**

`submission_status` reports the outcome — `success` once the verifier accepts the presentation, or `failed` if it is rejected. If the verifier returns a redirect target, it is included as `redirect_uri`.

```json
{
  "state": "3d8c088a-c23c-4038-923c-a1e9dabffc0e",
  "redirect_uri": null,
  "meta": { "submission_status": "success" }
}
```

You can review in-progress and completed flows for a wallet with `GET /wallets/{walletId}/send`, or a single flow with `GET /wallets/{walletId}/send/{state}`.
