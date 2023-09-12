# Vault Overview

The Meeco Vault is a secure storage solution where users can store a range of data types including attributes, documents, verifiable credentials (VCs), and digital media. The Meeco Vault is both [Privacy-by-Design (PbD)](/concepts/privacy-and-security-by-design.md) and [Security-by-Design (SbD)](/concepts/privacy-and-security-by-design.md). The Vault has a [Zero Knowledge Value (ZKV)](../../concepts/terminology.md#zero-value-knowledge-zvk) architecture, which means all data values are end-to-end encrypted with only the metadata in plain text.

By enabling plain text metadata an extensive range of use-cases are possible without accessing personally identifiable information (PII). For example, it is possible to request and exchange data without having direct access to it. It also allows for an extensible data schema and the addition of semantic labels.

All data values are encrypted in transit and at rest with unique cryptographic keys managed by the user of that Vault. Additionally, all data values are assigned a Universally Unique Identifier (UUID) to guard against correlation when the data is shared or exchanged. This approach also supports Progressive Disclosure and other data minimisation use cases.

To enable the sharing of data, the Meeco Vault comprises of four key elements: Slots, Items, Connections and Shares. Together, these elements provide users with flexibility, customisation and control of their data. Below is an introduction to these elements, for further details see our [Vault API documentation](/guides/api-guides/vault/README.md).

## Slots

Slots are the smallest data entity in the Vault. Each Slot represents a field that is seen by Vault users when creating an Item. Each Slot has a name (machine-readable non-empty string), a label, and a value, for example:
- Name: policy_number
- Label: Policy number
- Value*: Numerical

* Values can be strings, dates, or numbers, but also binaries such as images or documents. Values are always encrypted.

## Items

Items form the basis for every set of data stored in the Vault. In order for a user to store data (text, a document, an image etc.) they must first select an Item Template from the available options. These templates are made available by the ecosystem facilitator and will appear in user Vaults that are associated with the ecosystem. Templates allow users to categorise, sort and manage the data they store. They are a starting point and can easily be extended with other attributes. After selecting an Item Template, the Vault user can add descriptive information about the data being stored, for example:
- their house insurance policy number, and
- the date the policy is due to expire

They also have the option to attach files and assign tags (for ease of filtering). Once saved, a new Item will appear in the user's Vault.

## Connections

In order for Vault users to Share their Items, they must establish Connections with other Vault users. A Connection is established via an invitation flow which requires users to consent to the Connection being made. Connections can be cancelled at any time by either Vault user. Connections can also be governed by business rules, for example, maintain the Connection for the duration of being a customer, or until a service has been completed.

## Shares

The sharing of data occurs when a Vault user creates a Share and defines the following parameters:
- Which Item will be shared
- Whether all or specific Slots within an Item will be shared
- The Connection who will receive the share
- The length of time the Item will be shared
- Whether the Item can be On-Shared

When a Share is initiated by a Vault user, the recipient (Connection) must give consent to receiving the Share. This consent-based data sharing model ensures Vault users are not involuntarily receiving data from other users. This provides additional data governance and audit.

An additional feature to Shares is On-Shares where Vault users can specify if the recipient of a Share can then On-Share the Item with another Vault user. A key feature of On-Sharing is that the original Item owner maintains the Share parameters and any changes they make will propagate down through any On-Shares.

## Account Delegation

The Account Delegation feature enables Vault users to provide full access or read-only access to their data to a trusted individual. An example where this feature may be employed is when a Vault user (a child or an aging parent) requires assistance when managing or exchanging their data with other parties. With the Meeco Vault, they are able to ask a trusted individual to be a delegate of their Vault.

Note that in almost all cases we recommend using Shares and On-Shares to give access to other users as it gives much more granular control over the data shared to another user. However, there are circumstances where Account Delegation is more practical. Consider the use case(s) that you are developing when selecting different features.

# Implementation and Integration

Our Vault technology can be embedded in existing (front- and backend) applications such as mobile banking apps, web portals or back-up services with Authentication via an OIDC provider.

Convert data from on-boarding to any product or service to a Vault to enable customer control and direct collaboration. This approach minimises the amount of times a customer has to re-key information. It also increases security and decreases fraud and mistakes. Importantly, it provides a peer-2-peer connection with customers for secure communications, notifications and data sharing.

## End-user Vault

An end-user Vault is accessed, managed and controlled by one single user. End-user Vaults can be made available to participants within an ecosystem including, but not limited to: customers of a bank, students of an education institute, members of an association, or, individuals can manage their own Vault and establish Connections without being part of an established ecosystem.

## Enterprise Vault

An Enterprise Vault (EV) is a secure service similar to an end-user Vault but with added functionality tailored to enterprises. *Enterprise* refers to any entity, for example a company, government, or association. For further details on EVs, see the [Enterprise Vault](/platform/vault/enterprise-vault.md) information page.

# Standards and Compliance

For all European Union customers, data is hosted in Europe and is General Data Protection Regulation (GDPR) compliant. Speak to us about data sovereignty in other jurisdictions.
