# Selective Disclosure / ZKP

## Introduction
Firstly, it is worth noting that selective disclosure (SD) is a concept strongly linked to self-sovereign identity (SSI). SSI was established to bring awareness to digital users who lack control of their digital identity online. It is a model that provides individuals with the tools and information required to manage their identity data themselves, without relying on a third party. If we consider that SSI promotes users sharing their data with whomever they want, for however long, then users should also be able to choose how much or how little information they share. The ability to manage the amount of data being shared takes form as SD. An additional layer to SD is zero-knowledge proof (ZKP) cryptography which enables Verifiers to request proof of information rather than specific details. For example, a Verifier may want to know if a Holder is over the age of 18, rather than asking for the Holder’s birthday (which is personally identifiable information), they can simply ask for proof that the Holder is over the age of 18.

 

## Meeco’s ZKP Roadmap 
Meeco is currently in the process of developing ZKP capabilities via two pathways:

#### Pathway 01

The first pathway is based upon BBS+ Signatures that allow selective disclosure and holder blinding. The BBS+ Signatures is a multi-message digital signature; this allows us to share a piece of the verifiable credential and still prove its authenticity.

In this case, the Issuer needs to break up the single message into smaller pieces (or messages). The request for selective disclosure of certain attributes is done through the usage of JSON-LD frames, so that the Holder can reveal the information on an attribute level. The Issuer and the Holder require the usage of a BBS+ capable signature (e.g. BbsBlsSignatureProof).

#### Pathway 02

The second pathway enables more advanced ZKP techniques known as predicates, that allow us to derive statements that can be useful for age verification. This option is based upon the usage of SNARK Circuits.

The issuer builds a fixed-size Merkle tree, whose leaves represent the attributes of the Verifiable Credential. Each leaf of the Merkle tree is the hash of the attributes key and value (e.g. { “name”: “Alice” } -> Hash(“name” | “Alice”)). The tree is built deterministically, so that, starting from the same document, everyone can build the same tree. This allows us to use these attributes either as public inputs for the derived predicates or selectively disclose attributes.

An interim step, if this capability was required urgently, is to issue a specific additional credential(s) as part of an issuance flow. For example, a Birth Certificate could also have a standalone "over 18", "over 15", "over 21" credential issued at the time of original issuance. This would have to be re-issued when the status changed (i.e. on the Holder's 18th birthday where they would have to re-request the supplementary credentials).


