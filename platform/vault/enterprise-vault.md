# Enterprise Vault

An Enterprise Vault (EV) is a secure service similar to an End-user Vault with added functionality tailored to enterprises. _Enterprise_ refers to any entity, for example a company, government, or association. In the context of an EV, an “Organisation” is the entity that controls and manages their own EV.

## Functionalities

The key functionalities of an Enterprise Vault are as follows:
- [Items](/guides/api-guides/vault/items-and-slots.md), [Slots](/guides/api-guides/vault/items-and-slots.md), [Connections](/guides/api-guides/vault/connections-and-sharing.md), [Sharing](/guides/api-guides/vault/connections-and-sharing.md), [Classifications](/guides/api-guides/vault/classification-hierarchies.md), [Attachments](/guides/api-guides/vault/attachments.md) (as per an End-user Vault).
- Fined-grained consent capabilities to manage the sharing of data with end-users.
- The Organisation that manages an EV can have one or more Administrators who are authorised to onboard and manage other Administrators.
- The Administrator(s) of an EV can manage and deploy additional services that work harmoniously with the EV's features and functions.
- Administrators, with the associated access rights, have access to [Items](/guides/api-guides/vault/items-and-slots.md), [Connections](/guides/api-guides/vault/connections-and-sharing.md) and [Shares](/guides/api-guides/vault/connections-and-sharing.md) in the EV.
- Organisation Administrators have the ability to onboard third-party services (associated with the Organisation) to act on behalf of that Organisation. This enables third-party services access to [Items](/guides/api-guides/vault/items-and-slots.md), [Connections](/guides/api-guides/vault/connections-and-sharing.md) and [Shares](/guides/api-guides/vault/connections-and-sharing.md) within the EV in a secure, controlled way.
- An Organisation can connect to any user or other Organisation via a Vault-to-Vault conenction in order to undertake various workflows.

## Services executed via an Enterprise Vault

The following are examples of services the EV offers:
- **Secure data storage**: Storage of data including, but not limited to:
  - raw data
  - structured data
  - verifiable data
  - self-attested data
  - semantic data
  - verifiable and verified data (including Verifiable Credentials)
  - documents
- **Secure sharing**: Structured data shared in a persistent or one-off way, for example: a document or attachment. Shared data can also include a business rule that determines if the data can be edited/updated, or locked to prevent editing by the Organisation or other parties.
  - A specific implementation of secure sharing is the creation of an [Item](/guides/api-guides/vault/items-and-slots.md) pushed via a secure API to an End-user Vault, without storing the [Item](/guides/api-guides/vault/items-and-slots.md) in the EV. This allows the secure sharing of sensitive data from other systems without the need to maintain the [Item](/guides/api-guides/vault/items-and-slots.md) in the EV.
- **Securely receiving**: Structured data from any authorised party (end-user or third-party) within an EV ecosystem. This enables easy integrations with other services.

## Interacting with the Enterprise Vault

There are multiple ways to interact with and manage an EV, including:

**SVX Portal**: The [Portal](/platform/portal.md) is a web application that enables Organisation Administrators to onboard, invite additional Organisation Administrators, and begin accessing Meeco's Vault API via a low-code interface. When using the EV via the Portal, Administrators can view [Items](/guides/api-guides/vault/items-and-slots.md), [Connections](/guides/api-guides/vault/connections-and-sharing.md) and [Shares](/guides/api-guides/vault/connections-and-sharing.md). It also provides a view of the End-users and other third-party services that the Organisation is connected to.

**SVX API**: All functionality is available using the [SVX API (sandbox)](https://api-reference-sandbox.svx.exchange) to connect services.

**Command Line Interface (CLI)**: Meeco provides a [CLI](/tools/meeco-cli.md) to facilitate interactation with the EV.

**Onboarding to an Enterprise Vault**: To set up an EV, an authorised user must first create an Organisation. Note that setting up an Organisation will require either approval via the creation of a Meeco Licence, or via an ecosystem's Tenant Administrator.
