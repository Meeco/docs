# Decentralised Identifiers (DIDs)

Decentralised identifiers (DIDs) are a type of digital identifier that can be associated with any subject. The subject can be a: person, device, organisation or thing. Whatever type of subject the DID is linked to, they are controlled by the creator who can choose where and when to use them. The difference between other commonly used identifiers (e.g. email, twitter handle) and DIDs is that the former are controlled by someone else (email provider, Twitter) and can be taken away. DIDs solve that issue, which makes them a very important cornerstone for digital identity.

A DID may have one or more DID controllers who contribute to changes made to a DID document. Meeco's framework does not prohibit the usage of any property in the DID document by the DID controller, this is to aid future extensibility and versatility.

Key characteristics of a DID include:

* Decentralised: they are not managed or issued by a central authority.
* Persistent: once created, they are permanent and do not rely on an organisation to manage them (as opposed to DNS, email, etc).
* Cryptographically verifiable: designed to be associated with cryptographic keys and as such allow someone to prove control over the DID.
* Resolvable: allow any party to discover information about it to give it meaning.

See [Use Cases and Requirements for Decentralized Identifiers, W3C](https://www.w3.org/TR/did-use-cases/) for more information.

## DID Methods

There are many DID methods in use today (+200 at the time writing). Determining which DID method to use at any one time generally depends on its intended use case. An important consideration is whether the DID is going to identify a natural person (NP) or legal entity (LE). When identifying NPs, privacy regulations come into effect and, generally speaking, it is always safer to use a DID method that does not rely on a verifiable data registry (VDR). On the other hand, for LEs, it is rather useful to use a publicly accessible VDR that allows secure resolution of a DID.

DID method specifications are developed to define DID methods, including specific operations. These operations include how the DIDs and DID documents are created, resolved, updated, and deactivated.

### Universal Resolver

Meeco supports DID resolving for various DID methods via a system based on the Universal Resolver (UR). The UR has been designed and is managed by the [DIF Identifiers & Discovery Working Group](https://identity.foundation/working-groups/identifiers-discovery.html) and can be contributed to by individuals working in the discipline. The UR enables systems to resolve DIDs across many different DID methods. This allows system implementers to be DID-agnostic and provide seamless workflows between ecosystems.

## How are DIDs used?
While DIDs are used to identify a particular subject, personally identifiable information (PII) is not generally associated with the DID or associated DID document. Keeping in mind that a DID is a URI and as such a formatted combination of letters, numbers and specific symbols, a subject’s DID does not include the subject’s name, location or other identifying attributes. It is advised by W3C that, when PII associated with a subject needs to be managed or shared, it should be done so via a Verifiable Credential (VC) or service endpoints controlled by the DID subject. This process ensures that the privacy of the subject is maintained and the control over data sharing and exposure remains with the subject itself. Items such as VCs can be associated with DIDs to ensure the subject can maintain and manage their PII, however only the subject can read the data inside the VC. The subject can always share the VC with another party, and in that transaction the subject will grant access to the party to read their associated DID document.

For detailed information on Meeco’s DID implementations see our [DID API documentation](../guides/api-guides/dids/README.md).
