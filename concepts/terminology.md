# Terminology

## Claim
Attribute represented as a name-value pair.

## Classification
A link between a [Classification Node](#classification-node) and a classified entity. [Items](#item), [Slots](#slot) and [Templates](#item-template) can have Classifications.

## Classification Node
A [Classification Scheme](#classification-scheme) consists of a tree of _**Classification Nodes**_. A Classification Node:
- belongs to a Classification Scheme
- has a parent Classification Node, unless it is the root node
- has property `name`
- has property `label`
- has property `description`
- has property `image`

## Classification Scheme
Combinations of [Classifications](#classification) are called _**Classification Schemes**_.

## Connection
A persistent channel via which two entities can share information (e.g. [Items](#item), DIDs).

## Credential
For a comprehensive understanding of "Credential(s)", please refer to the [Verifiable Credentials](./verifiable-credentials.md) section.

## Credential Schema
A document that is used to guarantee the structure, and by extension the semantics, of the set of **claims** comprising a **Verifiable Credential**. A shared Credential Schema allows all parties to reference data in a known way. See [reference here](https://www.w3.org/TR/vc-data-model/#dfn-credential "https://www.w3.org/TR/vc-data-model/#dfn-credential").

## Credential Template
The defining properties of the resulting **Credential**. Credential Templates generally include:
- template name
- associated credential schema
- Issuer URL
- Issuer logo
- styling information (background and text colour)

## Data Encryption Key (DEK)
Are `AES256-GCM` keys used to encrypted and decrypt user data. They are stored in the [Keystore](#keystore) encrypted with the **Key Encryption Key**. It is possible for a user to have multiple Data Encryption Keys.

## Decentralized Identifier (DID)
[URIs](https://w3c.github.io/did-core/#dfn-uri) that associate a [DID subject](https://w3c.github.io/did-core/#dfn-did-subjects) with a [DID document](https://w3c.github.io/did-core/#dfn-did-documents) allowing trustable interactions associated with that subject. [DIDs](https://w3c.github.io/did-core/#dfn-decentralized-identifiers) have been designed so that they may be decoupled from centralized registries, identity providers, and certificate authorities. Specifically, the controller of a [DID](https://w3c.github.io/did-core/#dfn-decentralized-identifiers) can prove control over it without requiring permission from any other party.

## Derivation Artefact
Required in the process of generating or re-generating a **Passphrase Derived Key**. Derivation Artefacts include:
- Number of iterations
- Salt
- Derived key

## DID Subject
The entity identified by a DID and described by a DID Document. DID subjects include:
- people
- organizations
- physical entities
- digital entities

## Distributed Ledger Technology (DLT)
An umbrella term for technologies that provide distributed, append-only storage mechanics based on a consensus algorithm. Blockchain and hashgraph technologies are included under the term DLTs.

## Distributed Public Key Infrastructure (DPKI)
Same as PKI, but does not require a centralized authority to provide authenticity.

## Ecosystem
A group of organizations, users, and things that interact within a particular environment to achieve a (common) goal.

## End-to-end Encryption (E2E)
A system of communication where only the users communicating can read the messages. It prevents data from being read or secretly modified, other than by the true sender and recipient(s). The messages are encrypted by the sender (via the use of encryption keys), they are stored, encrypted, by the recipient, and are decrypted (read) by the recipient with another set of keys.

## End-users
A role within **SVX**. End-users, including **Wallet Holders**, partake in the exchange and sharing of data with **Issuers** and **Verifiers**. Via the use of Meeco’s Wallet application, they are able to, but not limited to:
- Register with **Tenants**
- **Connect** with **Organizations**
- Import credentials
- Import and respond to **Presentation Requests**

## Ephemeral DID
DID which is self-contained or generative, does not need to be represented in VDR.

## Issuer
A role an entity can perform by asserting claims about one or more subjects, creating a verifiable credential from these claims, and transmitting the verifiable credential to a holder. See reference here.

## Item
A group of [Slots](#slot) related by a topic. Common examples of Items:
- user profile
- club membership
- flight reservation
The Slots in an Item are keyed by their name property and contain only encrypted values.
Detailed documentation can be [found here](../guides/api-guides/vault/items-and-slots.md).

## Item Template
A predefined list of empty [Slots](#slot). Each Item is created by cloning a template and filling in the Slots with data.
Detailed documentation can be [found here](../guides/api-guides/vault/items-and-slots.md).

## JSON File Type
JSON is an open standard file format and data interchange format that uses human-readable text to store and transmit data objects consisting of attribute-value pairs and arrays. It is a common data format with diverse uses in electronic data interchange, including that of web applications with servers.

## JSON Web Tokens
JSON Web Token is a proposed Internet standard for creating data with optional signature and/or optional encryption whose payload holds JSON that asserts some number of claims. The tokens are signed either using a private secret or a public/private key.

## Key Encryption Key (KEK)
Used to encrypt all other keys (**Data Encryption Keys** and **Keypairs**) before they are stored in the [Keystore](#keystore). The Key Encryption Key is encrypted with the [Passphrase Derived Key](#passphrase-derived-key-pdk).

## Key Exchange
A process where at least two parties exchange cryptographic key(s) with the intention to use it/them for encryption or authentication.

## Keypair
A pair of private key(s) and public key(s) that are mathematically linked to each other. Public keys are used to encrypt data, and the private key of the keypair is used to decrypt that data. This is known as asymmetric encryption.

## Keystore
A component within SVX. The Keystore enables users to store and manage their cryptographic keys. This is where the [Data Encryption Keys](#data-encryption-key-dek), [Public/Private Keypairs](#keypair), and the [Key Encryption Key](#key-encryption-key) are stored along with the [Derivation Artefact](#derivation-artefact). All of the stored keys are encrypted with the **KEK**, except for the **KEK** itself, which is encrypted with the **Passphrase Derived Key**.
No encryption is done in the Keystore; the **Cryppo library** aids in creating and using keys.
Additional information can be [found here](../tools/cryppo.md).

## Organisation
An entity within **SVX**. An Organisation belongs to a Tenant and is managed by one or more **Organisation Administrators**.

## Organisation Administrator
A role within **SVX**. Organisation Administrators are individuals (users) who have administrator access and permissions to operate an **Organisation**. An Organisation Administrator is responsible for the actions that take place within their Organisation, including:
- Issuing credentials
- Verifying credentials
- Revoking credentials
- Creating and managing **Connections**

## Passphrase
A string of words that are used to authenticate a user when accessing a digital service or system. Passphrases are considered more secure than passwords as they are harder to decipher.

## Passphrase Derived Key (PDK)
A `PBKDF2` key. To generate or re-generate this key, a passphrase and derivation artefacts are required. Derivation artefacts include:
- Number of iterations
- Salt
- Derived key length

In the current iteration of our `Secret Key` authentication and passphrase derivation, the number of keys `Number of iterations` and `Derived key length` are static, and the Salt is pulled from the Secret Key.

## Personally Identifiable Information (PII)
Identifiers/attributes that may serve to uniquely identify a subject of the information.

## Presentation
Data derived from one or more [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/#dfn-verifiable-credentials), issued by one or more [Issuers](https://www.w3.org/TR/vc-data-model/#dfn-issuers), that is shared with a specific [verifier](https://www.w3.org/TR/vc-data-model/#dfn-verifier). See reference [here](https://www.w3.org/TR/vc-data-model/#dfn-credential).

## Presentation Definition
Presentation Definitions are objects that articulate what proofs a Verifier requires. These help the Verifier to decide how or whether to interact with a [Holder](https://identity.foundation/presentation-exchange/spec/v2.0.0/#term:holder). Presentation Definitions are composed of inputs, which describe the forms and details of the proofs they require, and optional sets of selection rules, to allow Holders flexibility in cases where many different types of proofs may satisfy an input requirement. See reference [here](https://identity.foundation/presentation-exchange/spec/v2.0.0/#term:presentation-definition).

## Presentation Request (PR)
Presentation Requests are transport mechanisms for [Presentation](https://identity.foundation/presentation-exchange/spec/v2.0.0/#term:presentation). Presentation Requests can take multiple shapes, using a variety of protocols and signature schemes not refined in this specification. They are sent by a [Verifier](https://identity.foundation/presentation-exchange/spec/v2.0.0/#term:verifier) to a [Holder](https://identity.foundation/presentation-exchange/spec/v2.0.0/#term:holder). Defining Presentation Requests is outside the scope of this specification. See reference [here](https://identity.foundation/presentation-exchange/spec/v2.0.0/#term:presentation-definition).

## Private Key (PrK)
A secret key in asymmetric cryptography used for decrypting ciphertext to plaintext.

## Public Key Infrastructure (PKI)
Infrastructure distributing cryptographic **public keys** based on a chain-of-trust, which is built around centralized authorities (entities issuing **Root Certificates**).

## Public Key (PuK)
A public key linked directly to a specific entity, used to encrypt plaintext into ciphertext, which can only be decrypted with the corresponding **Private Key**.

## Relying Party (RP)
An entity that relies upon the subscriber's **credentials**, typically to process a transaction or grant access to information or a system.

## Root Certificates
In cryptography and computer security, a root certificate is a public key certificate that identifies a root certificate authority (CA). Root certificates are self-signed and form the basis of an X.509-based public key infrastructure (PKI). See reference [here](https://en.wikipedia.org/wiki/Root_certificate "https://en.wikipedia.org/wiki/Root_certificate").

## Secret Key (also see **Private Key**)
In symmetric cryptography, a secret key (or "private key") is a piece of information or a framework used to decrypt and encrypt messages. Each party taking part in a transaction that is intended to be private possesses a common secret key. See reference [here](https://www.hypr.com/security-encyclopedia/secret-key). The secret key is a component of the authentication flow. The format for version 1 is as follows: `{version}-{username}-{salt}`. The `username` is generated by the server, and the `salt` is a 256-bit randomly generated key, which is base58 encoded and has a hyphen (-) at each 6th character. The salt component is created on the client and stored securely by the user. It is used to generate:
1. An encryption key [(aka Passphrase Derived Key (PDK))](#passphrase-derived-key-pdk) with which to encrypt your [Key Encryption Key (KEK)](#key-encryption-key-kek).
2. A password that, along with a username, will be used for [Secure Remote Password (SRP)](#secure-remote-password-srp) authentication.

## Secure Remote Password (SRP)
An authentication method that sends proof that a user knows their password without revealing the actual password to the server. Additional information can be found [here](https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol "https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol").

## Security Rights (SRs)
The permissions an individual user or a computer application holds to read, write, modify, delete, or otherwise access a computer file; change configurations or settings, or add or remove applications. See reference [here](https://soffront.com/glossary/access-rights/). Within **SVX**, one can differentiate between two types of security rights:
- External security rights, which are understood by other components in SVX.
- Internal security rights, which [ATOM](https://www.meeco.me/platform#component-atom) uses to manage itself.


## Security Rights Token (SRT)
A token that contains security rights assigned to a user or agent, which can be used as proof that it can perform certain actions.

## Share
A Share is created when a user grants access to one of their [Items](#item) to another user that they have [Connected](#connection) with. The **Item** is re-encrypted with a [Data Encryption Key](#data-encryption-key-dek) and shared with the recipient of the Share. An Item you have received via a Share can be shared with another user, but you cannot alter any of its **Slots**. Only the original creator of the Item can update the Share, other than deleting it. Detailed documentation can be found [here](../guides/api-guides/vault/connections-and-sharing.md).

## Slot
A Slot is the smallest data entity in the [Vault](#vault). An Item is made up of Slots, which are defined by the `name` property. Each Slot has a `name`, a `label`, and a `value`. Slots are able to be shared after two users have made a [Connection](#connection) with each other. Note that the API does not return the `value` property but `encrypted_value`. The API will not allow storing any unencrypted data in either `value` or `encrypted_value`. Slot values are always stored in an encrypted form, and only the user can decrypt and read them. Once encrypted and serialized, a Slot value of "BMW" would look something like this: `"encrypted_value": "Aes256Gcm.2hDl.LS0tCml2OiAhYmluYXJ5IHwtCiAgQWQwSThDZk5qRnFycmFuMAphdDogIWJpbmFyeSB8LQogIDJXVklzbUxOSWVoOHZIVDB1ZzBtZVE9PQphZDogbm9uQQo="`. Slots are typed, but the values cannot be checked to match the given type, as the API does not have decrypted keys for these items. Example Slot types are:
- `key_value`
- `bool`
- `date`
- `datetime`
- `image`
- `url`
- `phone_number`
- `email`
- `password`
- `attachment`
Notice that new types cannot be created; `key_value` should be the default type used.

## Software Development Kit (SDK)
A collection of software development tools in one installable package. They facilitate the creation of applications by having a compiler, debugger, and sometimes a software framework.

## Signing Key
See **Private Key**.

## Subject
A principal of a **Credential**. It can be a person, organization, thing, or entity.

## Secure Value Exchange (SVX)
Meeco's proprietary platform. [SVX](https://www.meeco.me/platform) provides customers with the building blocks to deliver trusted networks.

## (Verification) Submission
A term used within SVX but is identical to **Verifiable Presentation**.

## Tenant
An entity within [SVX](https://www.meeco.me/platform). A Tenant is operated by **Tenant Administrators** and is responsible for the governance of its **Tenancy** participants (including **Organisations** and **End-users**).

## Tenant Administrator
A role within [SVX](https://www.meeco.me/platform). Tenant Administrators are individuals (users) who have administrator access and permissions to operate a **Tenant**. A Tenant Administrator is responsible for the actions that take place within their Tenancy, including:

- Onboarding, managing and governing Organisations.
- Registering and managing **End-users**.

## Tenancy(ies)
A Tenancy is operated by an enterprise/company, referred to as a **Tenant** and consists of **Organisations**, and **End-users**.

## Uniform resource identifier (URI)
A Uniform Resource Identifier is a unique sequence of characters that identifies a logical or physical resource used by web technologies. URIs may be used to identify anything, including real-world objects, such as people and places, concepts, or information resources such as web pages and books.

## Universally unique identifier (UUID)
A number assigned to any type of data set or attribute to make it uniquely identifiable.

## Vault
Meeco’s Vault is where users can store and **Share** the [Items](#item) they create with **Connections** they establish. A Vault user’s data is **end-to-end encrypted** and is only accessible by them. Additional information can be found [here]().

## Verifiable Credential
A verifiable credential is a tamper-evident credential that has authorship that can be cryptographically verified. Verifiable credentials can be used to build [verifiable presentations](https://www.w3.org/TR/vc-data-model/#dfn-verifiable-presentations), which can also be cryptographically verified. The [claims](https://www.w3.org/TR/vc-data-model/#dfn-claims) in a credential can be about different [subjects](https://www.w3.org/TR/vc-data-model/#dfn-subjects). See reference [here](https://www.w3.org/TR/vc-data-model/#dfn-credential).

(and so on for the remaining terms)

## Verifiable Data Registry (VDR)
In the context of decentralised identity, is a place where Decentralised Identifiers (DIDs) can be anchored to.

## Verifiable Presentation
A tamper-evident presentation encoded in such a way that authorship of the data can be trusted after a process of cryptographic verification. Certain types of verifiable presentations might contain data that is synthesized from, but do not contain, the original [verifiable credentials](https://www.w3.org/TR/vc-data-model/#dfn-verifiable-credentials) (for example, zero-knowledge proofs). See reference [here](https://www.w3.org/TR/vc-data-model/#dfn-credential).

## Verification Request
See **Presentation Request**.

## Verification Template
The defining properties of the resulting **Presentation Request**. Verification Templates generally include:
- template name
- purpose (reason for requesting the specified **Credential(s)**)
- **Credential Schema(s)**

Verification Templates can be used repeatedly to form the basis of many different **Presentation Requests**.

## Verifier
A role an [entity](https://www.w3.org/TR/vc-data-model/#dfn-entities) performs by receiving one or more [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/#dfn-verifiable-credentials), optionally inside a [Verifiable Presentation](https://www.w3.org/TR/vc-data-model/#dfn-verifiable-presentations) for processing. Other specifications might refer to this concept as a relying party. See reference [here](https://www.w3.org/TR/vc-data-model/#dfn-credential).


## Verifying Key
A well-known key link directly to a specific entity. Used to confirm signatures. Technically, it is a public asymmetric key.

## Wallet

Software that enables the wallet’s controller (the end-user or **Wallet Holder**) to generate, store, manage and protect cryptographic keys and [Verifiable Credentials](#verifiable-credential). It allows the person to take actions (e.g. accept and present credentials) and setup peer-to-peer communication.

## Wallet Holder
An entity that stores and “owns” [Verifiable Credentials](#verifiable-credential). A Wallet Holder’s **Credentials** are cryptographically signed with the Holder’s signing key in the ‘holder’ section of the [Verifiable Credential](#verifiable-credential).

## Zero Knowledge Proof(s) (ZKP)
In cryptography, a zero-knowledge proof or zero-knowledge protocol is a method by which one party can prove to another party that a given statement is true while the prover avoids conveying any additional information apart from the fact that the statement is indeed true.

## Zero Value Knowledge (ZVK)
A system that has no knowledge (by using end-to-end encryption) of the data value, whilst allowing metadata to be accessible to the service. Metadata might include a data label (such as "street_name"), or classifications (such as "home").
