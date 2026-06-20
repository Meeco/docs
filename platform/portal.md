# Portal

The Secure Value Exchange (SVX) Portal is a web-based interface for platform and scheme administration. It is the primary tool for Tenant Administrators and Organisation Administrators to manage their tenancy structure, organisations, and application access.

As of SVX 4.0, the Portal is scoped exclusively to administrative and structural tasks. Credential and verification management (schemas, templates, issued credentials, presentation requests) has moved to the [Wallet Dashboard](/platform/wallet.md#wallet-dashboard), which is served directly by the Wallet service.

## Tenant context

When a Tenant Administrator logs in to the Portal, they operate within the tenant context. The available functions are:

**Dashboard** gives an overview of components and quick links to key sections.

**Organisations** lists all Organisations within the tenancy. Tenant Administrators can add new organisations, view and edit organisation details (name, URL, logo, associated credential schemas), archive organisations, and reinstate archived ones. See the [Manage Organisations tutorial](/guides/portal-tutorials/tenant-administrators/manage-organisations.md) for step-by-step instructions.

**Devtools** provides access to SVX documentation and the helpdesk for lodging service requests.

**Manage Tenancy** allows Tenant Administrators to manage their tenancy settings and invite or remove other Tenant Administrators. See the [Manage Tenancy](/guides/portal-tutorials/tenant-administrators/manage-tenancy.md) and [Manage Tenant Administrators](/guides/portal-tutorials/tenant-administrators/manage-tenant-administrators.md) tutorials for details.

**Manage Account** allows each administrator to update their own account settings.

## Organisation context

When an Organisation Administrator logs in to the Portal, they operate within the organisation context. The available functions are:

**Dashboard** gives an overview of the organisation's tools and quick links.

**Devtools** provides access to Applications (for M2M API access), SVX documentation, and the helpdesk.

**Applications** allows Organisation Administrators to create and manage application credentials (client ID and client secret) for machine-to-machine access to the SVX APIs. See the [Applications tutorial](/guides/portal-tutorials/organisation-administrators/applications.md) for details.

**Manage Organization** allows administrators to invite and remove Organisation Administrators and update account settings. See the [Manage Organisation Administrators tutorial](/guides/portal-tutorials/organisation-administrators/manage-organisation-administrators.md) for details.

**Manage Account** allows each administrator to update their own account settings.

## Authentication

Portal users authenticate using Meeco's secure login flow. On first access, Tenant Administrators are onboarded via an invitation sent by Meeco. Organisation Administrators are invited by their Tenant Administrator. See the [Onboarding tutorials](/guides/portal-tutorials/tenant-administrators/onboard-to-a-tenancy.md) for the full onboarding flow.

## Relationship to the Wallet Dashboard

The Portal and the Wallet Dashboard serve different audiences and purposes:

| Portal | Wallet Dashboard |
|---|---|
| Platform and scheme administration | Wallet operator management |
| Tenant and organisation structure | Credential schemas, templates, policies |
| Application API key management (org level) | Wallet API key management |
| Tenant and org admin accounts | Wallet admin accounts (passkey auth) |

Organisations that operate a Wallet deployment manage their credential workflows entirely within the Wallet Dashboard. The Portal does not provide credential or verification functionality in SVX 4.0.
