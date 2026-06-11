# Verification Templates

Verification templates define which credential(s) a Verifier requests and for what purpose.

SVX Wallet supports two flavours of verification template:
- [Digital Credential Query Language (DCQL)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-digital-credentials-query-l)
- [Presentation Exchange (PEX)](https://identity.foundation/presentation-exchange/spec/v1.0.0/)

A PEX verification template will include an `input_descriptors` array that describes the required credential(s).

A DCQL verification template will inclue a `dcql_query` object that describes the required credential(s).

## Who can undertake this operation?

Verification templates can only be created and managed by people who have access to the SVX Wallet Dashboard and API.

## Create Presentation Definition

Creation of a verification template

**Endpoint**

```bash
POST /verifier/templates
```

Use the following example to create a verification template with the Admin API:

DCQL
```bash
curl -X POST "$SVX_WALLET_BASE_URL/verifier/templates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "template": {
      "name": "dcql_template",
      "purpose": "identity verification",
      "type": "dcql",
      "dcql_query": {
        "credentials": [
          {
            "id": "pid",
            "format": "dc+sd-jwt",
            "meta": { "vct_values": ["https://credentials.example.com/identity_credential"] },
            "claims": [
              { "path": ["given_name"] },
              { "path": ["family_name"] },
              { 
                "path": [
                  "address", 
                  "street_address"
                ] 
              }
            ]
          }
        ]
      }
    }
  }'
```

PEX
```bash
curl -X POST "$SVX_WALLET_BASE_URL/verifier/templates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "template": {
      "name": "pex_template",
      "purpose": "identifty verification",
      "type": "pex",
      "input_descriptors": [
        {
          "id": "sd-jwt basic",
          "format": {
            "dc+sd-jwt": { "alg": ["ES256"] }
          },
          "purpose": "",
          "constraints": {
            "limit_disclosure": "preferred",
            "fields": [
              {
                "path": ["$.vct"],
                "filter": {
                  "type": "string",
                  "const": "ExampleCredentialHere"
                }
              },
              {
                "path": ["$.family_name"],
                "filter": { "type": "string" }
              },
              {
                "path": ["$.family_name"],
                "filter": { "type": "string" }
              }
            ]
          }
        }
      ]
    }
  }'
```

The API returns the created verification template object including its generated `id`.

## Remaining endpoints

The following endpoints are used to maintain and read verification template information:

- `GET /verifier/templates` - List verification templates available to the caller
- `GET /verifier/templates/{id}` - Read verification template by ID
- `PATCH /verifier/templates/{id}` - Update verification template by ID
- `DELETE /verifier/templates/{id}` - Delete verification template by ID