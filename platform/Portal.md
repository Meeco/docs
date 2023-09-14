The SVX Portal is a user-friendly interface that allows users to undertake processes enabled by the SVX API. Following an initial onboarding invitation, Tenant and Organisation Administrators can self-service in order to complete their desired workflows. Depending on their associated [Security Rights](./concepts/terminology.md#security-rights-(srs)), an Administrator can undertake the following tasks:

- Manage their Tenant / Organisation
- Invite and manage other Administrators
- Utilise the Credentials Service to:
  - Create and manage credential schemas
  - Create and manage credential templates
  - Issue and revoke credentials
  - Create and manage verification templates
  - Create and manage verification requests
- Utilise the Enterprise Vault to:
  - Create and manage connections
  - Manage items being shared
- Create and manage Applications

## Tenants

### Role, Administrators & Activity

The Portal enables enterprises to manage their own Tenancy. Once a Tenancy has been created, Tenant Administrators can onboard Organisations and invite Organisation Administrators to join. Tenant Administrators have access to [Credential Schemas](./guides/portal-tutorials/tenant-administrators/credential-schemas.md), [Applications](./guides/portal-tutorials/tenant-administrators/applications.md), [End Users](./guides/portal-tutorials/tenant-administrators/end-users.md), and management of their account.

> **Note**
> Once a Credential Schema is defined it can be made available to Organisations that will issue credentials. For more information on Credential Schemas see the [Credential Schema tutorial](./guides/portal-tutorials/tenant-administrators/credential-schemas.md).

### Responsibilities

During onboarding, all users of the Portal must agree to Meeco’s Terms and Conditions and Privacy Policy. It is the responsibility of the Tenant Administrator(s) to ensure the Organisations within their Tenancy are acting in an ethical, responsible manner.

## Organisations

### Role, Administrators & Activity

When Organisations are onboarded to the Portal they are invited to join an existing Tenancy. As mentioned above, the Tenant governs the Tenancy that the Organisation conducts business in. Organisation Administrators manage the Organisation and, depending on their [Security Rights](./concepts/terminology.md#security-rights-(srs)), can action workflows associated with Credentials, their Enterprise Vault, Applications and account settings. 

When utilising credentials, Organisations can act as Issuers and/or Verifiers, thus allowing them to manage:
- Credential Templates
- Issue, revoke and verify credentials
- Verification Templates
- Verification Requests

Each time an action is undertaken, Organisation Administrators can view and / or archive the item / occurrence. This provides Organisations with fine-grained reporting and transaction management capabilities.

### Responsibilities

During onboarding, all users of the Portal must agree to Meeco’s [Terms and Conditions](https://www.meeco.me/terms) and [Privacy Policy](https://www.meeco.me/privacy). It is the responsibility of the Organisation Administrator(s) to ensure all Administrators are acting in an ethical, responsible manner.

## Credentials

The Portal provides access to credential management in a simple, user-friendly way. It enables non-technical users to engage with our Verifiable Credential workflows via an intuitive interface. Our credential service utilises the [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model-2.0/) to ensure standardisation and interoperability with  external systems. Additionally, when undertaking credential-based workflows via the Portal, users are engaging in industry leading privacy and security practices that are aligned with current specifications and regulations, specifically the Australian Privacy Principles and General Data Protection Regulation (GDPR) (for details see our [Privacy- and Security-by-design](./concepts/privacy-and-security-by-design.md) page).

### Credential Schemas

The Portal provides Tenant Administrators with the option to create Credential Schemas within their Tenancy and make them available to Organisations. With access to a library of Credential Schemas, Organisations can specify which schema is required when creating Credential and Verification Templates. When Organisations within the same ecosystem are referencing the same schemas, credential issuance and verification is accurate and standardised. For further details on how to create and manage Credential Schemas, see the [Credential Schemas tutorial](./guides/portal-tutorials/tenant-administrators/credential-schemas.md). 

### Credential Templates

Organisations that issue credentials (Issuers) can create Credential Templates which form the base of the credentials they will issue. Credential Templates include the associated credential schema (which determines the information required from the Holder), and credential styling information which enables customisation of credential colour schemes and logos. For further details on how to create a Credential Template, see the [Credential Template tutorial](./guides/portal-tutorials/organisation-administrators/credential-templates.md). 

### Issue credentials

The issuance of credentials is made easy via the Portal by a simple step-by-step workflow. Issuers first select one of the Credential Templates they have created previously (see the [Credential Template tutorial](./guides/portal-tutorials/organisation-administrators/credential-templates.md) for details). The Credential Template defines the attributes that are required to be filled in. For example, a First Aid Certificate Credential Template might include attributes such as:
- Student name
- Student ID
- Course ID
- Subject code
- Grade

Once the Organisation Administrator has filled in all required attributes specific to the credential recipient (Holder) they are able to issue the credential. For further details on how to issue a credential, see the [Issue / Revoke Credentials tutorial](./guides/portal-tutorials/organisation-administrators/issue-revoke-credentials.md). 

### Revoke credentials

When revoking a credential, an Organisation Administrator simply locates the credential in the list of issued credentials, and selects the option to revoke. For further details on how to revoke a credential, see the [Issue / Revoke Credentials tutorial](./guides/portal-tutorials/organisation-administrators/issue-revoke-credentials.md). 

### Verification Templates

Organisations that verify credentials (Verifiers) can create Verification Templates which form the base of Verification Requests. Verification Templates include the associated credential schema (which determines the information required from the Holder), and if required, a specific Issuer of these credentials can be defined. For further details on how to create a Verification Template, see the [Verification Template tutorial](./guides/portal-tutorials/organisation-administrators/verification-templates.md).

> **Note**
> Please note that we acknowledge the term [Verifiable Presentation](./concepts/terminology.md#verifiable-presentation) as commonly used in credential specifications, but for the purposes of our Portal, we refer to Verifiable Presentations as Verification Templates.

### Verification Requests

In order to verify credentials via the Portal, an Organisation Administrator (Verifier) creates a new Verification Request. Each request requires the Organisation Administrator to select a Verification Template which defines the credential(s) required from the Holder for verification. Once a Verification Template has been selected, the Verifier can view the request in the form of a QR Code. The QR Code can be presented to a Holder in-person, or downloaded and shared via any form of digital sharing method. For further details on how to create a Verification Requests, see the [Verification Requests tutorial](./guides/portal-tutorials/organisation-administrators/verification-requests.md). 

## Additional Credential Service Information

For detailed information about our credential service, see our [Verifiable Credentials](https://www.meeco.me/verifiable-credentials) information page or our [API documentation](https://api-reference-sandbox.svx.exchange/).

## Enterprise Vault

While the Enterprise Vault (EV) can be directly utilised via the SVX API, it can also be managed by Organisation Administrators via the Portal. By providing an interface to the EV, non-technical Administrators can easily create and manage connections, including the sharing of items on behalf of their Organisation. For further information, see the [Enterprise Vault](./platform/vault/enterprise-vault.md) overview page.

### Connections

The EV allows Organisations to securely share data with end users via Vault-to-Vault connections. Organisation Administrators can invite end users to connect via a simple invitation workflow. After entering the connection’s full name into the system, they can generate an invitation code. This invitation code is then passed to the new connection which they enter into their own Vault-using application. On acceptance by the connection recipient, the connection between the two Vaults is established. For further details on establishing and managing connections, see the [Connections tutorial](./guides/portal-tutorials/organisation-administrators/connections.md).

## Additional Enterprise Vault Information

For detailed information about our EV offering, see the [Enterprise Vault](./platform/vault/enterprise-vault.md) information page or our [API documentation](https://api-reference-sandbox.svx.exchange/).
