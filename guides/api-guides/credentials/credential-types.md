# Credential Types

Credential types enable Organisations to select a [Credential Schema](credential-schemas.md) when preparing credentials to be issued. They also allow Organisations to define the visual appearance of the credential which can reflect the Organisation's branding requirements. Credential types are used in both the Meeco Enterprise Portal. Please note that in the Enterprise Portal, credential types are referred to as Credential Templates.

## Prerequisites

- [Credential Schema](credential-schemas.md)

## Who can undertake this operation?

Credential types are created and managed by an Organisation Administrator.

## Create Credential Type

Creation of a credential type.

**Endpoint**

```bash
POST /credential_types
```

**Request body**

```bash
{ "credential_type":
  { "name":{name},
    "schema_id":{credential_schema_id},
    "format":{format},
    "config":{config},
    "style":{"text-color":{text_color},"background":{background_color}, "logo": {logo}}    
  }
}
```

- `name` – Name of the credential type
- `credential_schema_id` - The associated credential schema ID
- `format` - Format of the credential i.e. sd+vc-json
- `config` - This attribute allows to define disclosure_frame, vct, expiry, issuer_credential_configurations_supported_name (optional)
- `style`
  - `text_color` – Text colour for the credential (CSS styles supported)
  - `background_color` – Background colour for the credential (CSS styles supported)
  - `logo` – Logo displayed in the top left corner of the credential (optional)

**Response**

The credential type object that is created.
