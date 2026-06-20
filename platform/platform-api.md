# Platform API

The Platform API is the backend service that manages the tenancy and organisation structure of an SVX deployment. It handles authorisation for all platform participants, maintains the hierarchy of Tenants and Organisations, and enables machine-to-machine access for applications.

The [Portal](/platform/portal.md) is a web UI built on top of the Platform API, providing Tenant and Organisation Administrators with a visual interface for the same capabilities.

## Tenants and Organisations

The Platform API maintains a two-level hierarchy: Tenants contain Organisations. A Tenant represents a scheme operator or an enterprise running an SVX deployment. Organisations are participants within that scheme, typically acting as credential Issuers, Verifiers, or both.

Each level has its own Administrators with scoped permissions:

- **Tenant Administrators** manage the tenancy: they add and remove Organisations, invite other Tenant Administrators, and configure tenancy-level settings such as which credential schemas are available to Organisations.
- **Organisation Administrators** manage a single Organisation: they invite other Organisation Administrators and create application credentials for M2M API access.

See [Tenants, Organisations and End-Users](/platform/tenants-organisations-and-end-users.md) for a full description of all participant roles.

## Application API credentials

Organisations that need programmatic access to the SVX APIs create application credentials (client ID and client secret) via the Platform API or through the Applications section of the Portal. These credentials are used in the OAuth 2.0 client credentials flow to obtain a bearer token for API requests.

## Event logging

The Platform API records events for actions taken within the system, providing an audit trail for compliance and accountability purposes.

## SVX 4.0 scope

In SVX 4.0, the Platform API is focused on scheme structure and administration. Credential and verification management has moved to the [Wallet Service](/platform/wallet.md). Additional Platform API capabilities will be introduced in subsequent minor releases under SVX 4.x.
