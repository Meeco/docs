## Introduction

An _end user_ is a role within SVX. End users include people and entities that manage a digital wallet and are commonly referred to as _Holders_. Holders partake in the exchange of data with Issuers and Verifiers. When using Meeco's Holder Wallet API, including front-end applications built on our wallet technology, Holders are able to: 
* Onboard with a Tenant 
* Interact with Organisations (Issuers and Verifiers)
* Add credentials 
* Add and respond to Verification Requests

## Onboard an end user to a Tenancy

As the Portal is designed to enable Tenant Administrators to view activity undertaken within their Tenancy, the onboarding of end users is managed directly via the SVX API. To do this, Tenant Administrators must [create an Application](./applications.md) to enable the creation of end user wallets and ensure these wallets can communicate with all SVX services.

## View an end user’s details

Tenant Administrators can view an end user’s details by navigating to _Manage Tenancy_ on the left-side menu of the SVX Portal, and selecting _End users_. Locate the end user in the list and select the horizontal ellipsis icon ⋯ alongside the end user entry to reveal menu options. Select _View_. You will be presented with the following information:
* User DID
* Date registered

## Remove an end user from a Tenancy

To remove an end user from a Tenancy navigate to _Manage Tenancy_ on the left-side menu of the SVX Portal, and select _End users_. Locate the end user in the list and select the horizontal ellipsis icon ⋯ alongside the end user entry to reveal menu options. Select _Cancel and remove_ and confirm the removal of the end user via the modal window. The end user will be removed from the Tenant and will no longer appear in the list of end users.

> **Note**
> When an end user is removed from a Tenancy any credentials that have been issued to the end user will remain in their wallet. Similarly, all Verification Requests (Pending and completed) will remain in the Holder's wallet.

## Restore an end user to a Tenancy

If a Tenant Administrator provides an end user with a newly generated invitation QR Code within 30 days of the end user being removed from the Tenancy, the end user will have their end user vault reinstated and will be able to restore their connections with Organisations.
