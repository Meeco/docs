# Verification Request

SVX Wallet supports verification request via [OpenID 4 Verifiable Presentation (OID4VP)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)

A verification request is an [OAuth 2.0 Authorization Request](https://www.rfc-editor.org/info/rfc6749).

It can be generated based on an existing [Verification Template](./verification-templates.md).

The resulting Authorization Request object will convey the required credential. 

SVX Wallet supports verification of presentation responses via the following response modes:
- `direct_post`
- `direct_post.jwt`

SVX Wallet supports the verifiable presentations of the following formats
- `jwt_vc_json`
- `dc+sd-jwt`
- `mso_mdoc`

## Who can undertake this operation?

Verification requests can only be created by people who have access to API.
It can be managed by people who have access to the Dashboard and the API.

## Generate Verification Request

Creation of a verification request

**Endpoint**

```bash
POST /verifier/requests
```

Use the following example to create a verification request with the Admin API:

```bash

curl -X POST "$SVX_WALLET_BASE_URL/verifier/templates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
   "name": "authorization request",
   "presentation_template_id": "my-verification-template-id",
   "response_mode": "direct_post",
   "verifier_redirect_uri": "https://verifier.example.com/callback",
   "expected_origins": [
      "https://verifier.example.com"
   ]
  }'
```

The API returns the created verification request including its generated `id`.

The verification request object JWT can then be retrieved from the following URL.

`GET {SVX_WALLET_BASE_URL}/verifier/requests/{id}/jwt`

This URL can be used to deliver the request to the requestee instead of the request object.


The requestee is expected to send a response to the following endpoint:

`POST {SVX_WALLET_BASE_URL}/verifier/requests/{id}/responses`

## Remaining endpoints

- `GET /verifier/requests` - List all available verifier requests
- `GET /verifier/requests/{id}` - Read verifier request by ID
- `DELETE /verifier/requests/{id}` - Read verifier request by ID
- `GET /verifier/requests/{id}/responses` - List all received presentation responses for a verification request
- `GET /verifier/requests/{id}/responses/{responseId}` - Read presentation response by ID
- `DELETE /verifier/requests/{id}/responses/{responseId}` - Delete stored presentation response by ID