# Credential Templates

Credential templates enable SVX Wallet administrators to select a [Credential Schema](credential-schemas.md) and define which credentials a wallet can issue. Templates can also define format-specific attributes, credential issuer metadata, as well as common presentation and configuration settings.

Format-specific configuration options are: `dc+sd-jwt` (`config.vct`, `config.disclosure_frame`), `mso_mdoc` (`config.doctype`, `config.disclosure_frame`), and `jwt_vc_json` (`config.types`). Other template options are common across formats.

## Prerequisites

- [Credential Schema](credential-schemas.md)

## Who can undertake this operation?

Credential templates can only be created and managed by people who have access to the SVX Wallet Dashboard and API.

## Create Credential Template

Creation of a credential template.

**Endpoint**

```bash
POST /issuer/templates
```

Use the following example to create a credential template with the Admin API:

```bash
curl -X POST "$SVX_WALLET_BASE_URL/issuer/templates" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "template": {
      "name": "Email Credential Template",
      "schema_id": "<credential_schema_id>",
      "config": {
        "vct": "com.meeco.credentials.email.v1"
      },
      "style": {
        "text": "#FFFFFF",
        "background": "#1E3A8A",
        "logo": "<image_asset_id>"
      }
    }
  }'
```

The API returns the created credential template object, including its generated `id`.

## Remaining endpoints

The following endpoints are used to maintain and read credential template information:

- `GET /issuer/templates` - List credential templates available to the caller.
- `GET /issuer/templates/{id}` - Read credential template by ID.
- `PUT /issuer/templates/{id}` - Update credential template by ID.
- `DELETE /issuer/templates/{id}` - Delete credential template by ID.
