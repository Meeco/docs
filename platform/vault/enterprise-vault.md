# Enterprise Vault

An Enterprise Vault (EV) is a secure service similar to an end-user Vault with added functionality tailored to enterprises. _Enterprise_ refers to any entity, for example a company, government, or association. In the context of an EV, an “Organisation” is the entity that controls and manages their own EV.

## Functionalities

The key functionalities of an Enterprise Vault are as follows:
- [Items](docs/guides/vault/items-and-slots.md), [Slots](docs/guides/vault/items-and-slots.md), [Connections](docs/guides/vault/connections-and-sharing.md), [Sharing](docs/guides/vault/connections-and-sharing.md), [Classifications](docs/guides/vault/classification-hierarchies.md), [Attachments](docs/guides/vault/attachments.md) (as per an end-user Vault).
- The Organisation that manages an EV can have one or more Administrators who are authorised to onboard and manage other Administrators.
- The Administrator(s) of an EV can manage and deploy additional services that work harmoniously with the EV's features and functions.
- Administrators, with the associated access rights, have access to [Items](docs/guides/vault/items-and-slots.md), [Connections](docs/guides/vault/connections-and-sharing.md) and [Shares](docs/guides/vault/connections-and-sharing.md) in the EV. 
- An Organisation can connect to any user or other Organisation via a Vault-to-Vault conenction in order to undertake various workflows.
- Fined-grained consent capabilities to manage the sharing of data with end users.
- Organisation Administrators have the ability to onboard third-party services (associated with the Organisation) to act on behalf of that Organisation. This enables third-party services access to [Items](docs/guides/vault/items-and-slots.md), [Connections](docs/guides/vault/connections-and-sharing.md) and [Shares](docs/guides/vault/connections-and-sharing.md) within the EV in a secure, controlled way.

## Services executed via an Enterprise Vault

The following are examples of services the EV offers:
- _Secure data storage_ - This data includes, but is not limited to:
  - raw data
  - structured data
  - verifiable data
  - self-attested data
  - semantic data
  - verifiable and verified data (including Verifiable Credential)
  - documents
- _Secure sharing_ - Structured data shared in a persistent or on-off way, for example: a document or attachment. Shared data can also include a business rule that determines if the data can be edited/updated, or locked to prevent editing by the Organisation or other parties.
  - A specific implementation of secure sharing is the creation of an Item pushed via a secure API to an end-user Vault, without storing the item in the EV. This allows the secure sharing of sensitive data from other systems without the need to maintain the Item in the EV.
- _Securely receiving_ - Structured data from any authorised party (end-user or third party) within an EV ecosystem. This enables easy integrations with other services.

## Interacting with the Enterprise Vault

There are multiple ways to interact with and manage an EV, including:
**Meeco's Enterprise Portal**
The Enterprise Portal is an application that enables Organisation Administrators to onboard, invite additional Organisation Administrators, and begin accessing Meeco's Vault API via a low-code interface. When using the EV via the Enterprise Portal, Administrators can create, view, and manage [Items](docs/guides/vault/items-and-slots.md), [Connections](docs/guides/vault/connections-and-sharing.md) and [Shares](docs/guides/vault/connections-and-sharing.md). It also provides a view of the end-users and other third-party services that the Organisation is connected to.

**SVX API**
All functionality is available using the [SVX API](https://dev.meeco.me/api-details#api=meeco-vault-api&operation=put-delegation_invitations-id-accept) to connect services.

**Command Line Interface (CLI)**
Meeco provides a [CLI](docs/tools/meeco-cli.md) to facilitate interactation with the EV.

**Onboarding to an Enterprise Vault**
To set up an EV, an authorised user must first create an Organisation. Note that setting up an Organisation will require either approval via the creation of a Meeco Licence, or via an ecosystem's Tenant Administrator.
