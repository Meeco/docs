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

## Prerequisites
- [Verification Template](./verification-templates.md)

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

curl -X POST "$SVX_WALLET_BASE_URL/verifier/requests" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
   "name": "Age over 18 check request",
   "presentation_template_id": "my-verification-template-id",
   "response_mode": "direct_post",
   "verifier_redirect_uri": "https://verifier.example.com/callback"
  }'
```

The API returns the created verification request including its generated `id`. <br>

The verification request object JWT can then be retrieved from the following URL. <br>

`GET {SVX_WALLET_BASE_URL}/verifier/requests/{id}/jwt`

The above URL delivers a JWT-Secured Authorization Request [(JAR)](https://www.rfc-editor.org/info/rfc9101/) wth the `typ` header value `oauth-authz-req+jwt`. <br/>

The request also includes a `client_id` that with a prefix that can be used to specify the mechanism to obtain and validate the metadata of the verifier.

For example:

```
{
   "client_id": "x509_san_dns:verifier.example.org",
   "response_type": "vp_token",
   "nonce": "...",
   "dcql_query": { ... }
}
```


The requestee is expected to send a response to the following endpoint:

`POST {SVX_WALLET_BASE_URL}/verifier/requests/{id}/responses`

The SVX Wallet supports the following response_types:
- `direct_post`
- `direct_post.jwt` for encrypted responses

### `direct_post`

Example of a presentation response sent to the Verifier (SVX Wallet).
```
POST /verifier/requests/{id}/responses HTTP/1.1
Host: SVX_WALLET_BASE_URL
Content-Type: application/x-www-form-urlencoded

  vp_token=...&
  state=eyJhb...6-sVA
```

### `direct_post.jwt` - encrypted response

The required information for the requestee to derive the shared secret are included in the request object.<br>
And similarly for the verifier as a requestor, the information to derive the shared secret is included in the encrypted response.

Example of an encrypted presentation response sent to the verifier (SVX Wallet).
```
POST /verifier/requests/{id}/responses HTTP/1.1
Host: SVX_WALLET_BASE_URL
Content-Type: application/x-www-form-urlencoded

response=eyJra...9t2LQ
```

An example of the decrypted JWT payload would be:
```
{
  "vp_token": {"example_jwt_vc": ["eY...QMA"]}
}
```
Which is similar to what is sent in a `direct_post` response mode.


## Remaining endpoints

- `GET /verifier/requests` - List all available verifier requests
- `GET /verifier/requests/{id}` - Read verifier request by ID
- `DELETE /verifier/requests/{id}` - Read verifier request by ID
- `GET /verifier/requests/{id}/responses` - List all received presentation responses for a verification request
- `GET /verifier/requests/{id}/responses/{responseId}` - Read presentation response by ID
- `DELETE /verifier/requests/{id}/responses/{responseId}` - Delete stored presentation response by ID