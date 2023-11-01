## Introduction

An _end user_ is a role within SVX. End users include Wallet Holders and entities that partake in the exchange of data with Issuers and Verifiers. Wallet Holders using Meeco’s Wallet application are able to: 
* Register with Tenants 
* Connect with Organisations
* Import credentials 
* Import and respond to Presentation Requests 

Tenant Administrators are responsible for registering end users with their Tenancy to enable these workflows.

## Register an end user with a Tenancy

Tenant Administrators can register an end user with their Tenancy by navigating to _Manage Tenancy_ on the left-side menu of the SVX Portal, and selecting _End users_. Next, select the _Generate invitation QR code_ button. You will be presented with an Invitation QR Code. You are able to download the QR Code and copy the QR Code link for distribution to end users.

Select the _Done_ button to close the _Generate invitation QR Code_ screen.

The list of end users contains a status label for each registration:
* Pending (invitation is waiting to be actioned by the end user)
* Registered (invitation has been accepted by the end user)
* Expired (invitation has not been used within the expiration period of 30 days)

The registration of an end user with a Tenancy includes the creation of an end user vault. This vault enables the end user to create connections with Organisations and share credentials.

> **Note**
> _Invitation QR Codes:_ Generated QR Codes can only be used once per end user.

> **Note**
> _Expiration date:_ The standard invitation expiry period is 30 days. This parameter can be changed via the use of the SVX API.

![How to register an end user with a Tenancy tutorial video.](/.gitbook/assets/19_TA_Register_an_End_User.gif)

## View an end user’s details

Tenant Administrators can view an end user’s details by navigating to _Manage Tenancy_ on the left-side menu of the SVX Portal, and selecting _End users_. Locate the end user in the list and select the horizontal ellipsis icon ⋯ alongside the end user entry to reveal menu options. Select _View_. You will be presented with the following information:
* The invitation QR Code (pending invitations only)
* User DID
* Invitation code
* Data invitation generated
* Date registered
* Status

![How to view an end user's details tutorial video.](/.gitbook/assets/20_TA_View_End_Users_details.gif)

## Remove an end user from a Tenancy

To remove an end user form a Tenancy navigate to _Manage Tenancy_ on the left-side menu of the SVX Portal, and select _End users_. Locate the end user in the list and select the horizontal ellipsis icon ⋯ alongside the end user entry to reveal menu options. Select _Cancel and remove_ and confirm the removal of the end user via the modal window. The end user will be removed from the Tenant and will no longer appear in the list of end users.

> **Note**
> When an end user is removed from a Tenancy their associated end user vault is deactivated. Because of this, any connections between the end user and Organisations are cancelled. Any credentials that have been issued to the end user will remain in their wallet.

![How to remove an end user from a Tenancy tutorial video.](/.gitbook/assets/21_TA_Remove_End_User.gif)

## Restore an end user to a Tenancy

If a Tenant Administrator provides an end user with a newly generated invitation QR Code within 30 days of the end user being removed from the Tenancy, the end user will have their end user vault reinstated and will be able to restore their connections with Organisations.

![How to restore an end user to a Tenancy tutorial video.](/.gitbook/assets/22_TA_Reconnect_with_End_User.gif)
