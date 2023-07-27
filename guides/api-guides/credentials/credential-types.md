# Credential Types

Credential types enable Organisations to select a [Credential Schema](credential-schemas.md) when preparing credentials to be issued. They also allow Organisations to define the visual appearance of the credential which can reflect the Organisation's branding requirements. Credential types are used in both the Meeco Enterprise Portal and Meeco Wallet. Please note that in the Enterprise Portal, credential types are referred to as Credential Templates.

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

**Request**

* Name – Name of the credential type
* Credential Schema - The associated credential schema
* Style
  * Text Colour
  * Background Colour – Background colour for the credential (CSS styles supported)
  * Logo – Logo displayed in the top left corner of the credential

**Response**

The credential type object that is created.
