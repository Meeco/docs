# Verifiable Credentials

Digital credentials are becoming more commonly used to assert and transfer information about individuals, organisations and things. A credential is a structred document that that contains key-value pairs (referred to as claims). W3C Verifiable Credentials (VCs) is a specification that enable individuals, organisations and things to share credentials with third-parties to prove they poses certain attributes, knowledge and permissions in a machine-verifiable way.

VCs are compiled of a set of claims made about a Holder / Subject. They can be imagined as digital equivalents of physical documents (e.g. driver licences, passports, birth certificates). What differentiates them from physical documents and other non-secure forms of digital data is that they are tamper-resistant and can be cryptographically verified. In addition to this, VCs:

* Are based on international standards such as those published by the [W3C](https://www.w3.org/TR/vc-data-model/)
* Include decentralised identifiers [DIDs](../platform/did.md). (for all actors)
* Are designed to be globally interoperable
* Require cryptographic proof throughout the credential lifecycle
* Can be used across ecosystems
* Adhere to a common schema
* Utilise common / shared lists for resolving and revocation

## The VC Ecosystem

### Issuer, Holder and Verifier

Inside the VC Ecosystem there are three distinct roles: an *Issuer*, a *Verifier*, and a *Holder*. At the centre of the ecosystem is the Holder (not to be confused with the subject although these are often the same). The exchange of VCs starts with the Issuer who generates the credential and issues it to the holder. The VC contains:

* Set of claims about the Holder (e.g. name, date of birth)
* Issuer’s signature
* Credential metadata (e.g. expiry date)
* Credential identifier (a unique number)

Once issued, Holders can store their VCs in a digital wallet. In the event a Holder needs to, for example, provide evidence of their identity, a skill, access rights . They can share proof of one or more claims stored within their VC(s) to actors inside the ecosystem called Verifiers via a Verifiable Presentation. A Verifiable Presentation is the requested information (VCs) packaged by the Holder and presented to a Verifier. These presentations are verified by the receiving party in order to perform a service.

### Verifiable Data Registry

A verifiable data registry (VDR) is a role undertaken by a system within a VC Ecosystem. The purpose of this role is to mediate the actions undertaken by the actors in the ecosystem. These actions include the creation and verification of identifiers, keys, VC schemas etc. VDRs take many forms, depending on the type of ecosystem being orchestrated will determine which VDR should be used. When sharing and exchanging VCs, the W3C trust model only requires that “all entities trust the verifiable data registry to be tamper-evident and to be a correct record of which data is controlled by which entities.” The different types of VDRs include:

* Trusted databases - a database governed by one or more organisations that is managed centrally, across multiple systems or on a cloud.
* Ledgers - a verifiable transaction log where entries are only ever added, not removed from the ledger.
  * Centralised ledgers - managed by a ledger operator and does not require a consensus mechanism.
  * Distributed ledgers - a ledger that is replicated across many systems or entities, where transactions are updated in unison. Requires a consensus mechanism.
  * Blockchains - uses cryptographically linked blocks of data that build as more transactions occur.

Meeco’s products are VDR agnostic, any of the above VDR types can be used when creating and managing a VC Ecosystem with Meeco technology.

### Digital Wallets

[Digital wallets](../concepts/digital-wallets.md) enable users to manage their digital assets easily and securely from the device(s) of their choosing. Wallets are commonly used for the management of data and transactions, ranging from identity documentation (driver licences and certificates), financial (payments and transfers), tokens (fungible and non-fungible), and loyalty programs (vouchers and point accumulation). To better understand how VCs are managed in digital wallets, see our Digital Wallets information page.

## VCs in real-world applications

To see how Verifiable Credentials can be used in different workflows, please view Meeco’s case studies and existing [customer applications](https://www.meeco.me/powered-by-meeco) to find out more.
