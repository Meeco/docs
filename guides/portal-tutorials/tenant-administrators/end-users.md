## Introduction

An _end user_ is a role within SVX. End users include Wallet Holders and entities that partake in the exchange of data with Issuers and Verifiers. Wallet Holders using Meeco’s Wallet application are able to: 
* Register with Tenants 
* Connect with Organisations
* Import credentials 
* Import and respond to Presentation Requests 

Tenant Administrators are responsible for registering end users with their Tenancy to enable these workflows.

## Register an end user with a Tenancy

Tenant Administrators can register an end user with their Tenancy by navigating to _Manage Tenancy_ on the left-side menu of the SVX Portal, and selecting _End users_. Next, select the _Generate invitation QR Code_ button. You will be presented with an Invitation QR Code. You are able to download the QR Code and copy the QR Code link for distribution to end users.

Select the _Done_ button to close the _Generate invitation QR Code_ screen. Prior to the end user scanning the QR Code, the pending invitation will appear under the _Pending Invitations_ tab.

The end users table contains three tabs:
* Active End Users (end users who have registered with the Tenant).
* Pending Invitations (invitations that are waiting to be actioned by the end user).
  * Expired Invitations appear in the _Pending Invitations_ tab if the invitation has not been used within the expiration period of 30 days.
* Removed End Users (end users that have been removed from the Tenancy).

The registration of an end user with a Tenancy includes the creation of an end user vault. This vault enables the end user to create connections with Organisations and share credentials.

> **Note**
> _Invitation QR Codes:_ Generated QR Codes can only be used once per end user.

> **Note**
> _Expiration date:_ The standard invitation expiry period is 30 days. This parameter can be changed via the use of the SVX API.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_19_Register_an_End_User.gif" alt="How to register an end user with a Tenancy tutorial video." width="80%">
</p>

## View an end user’s details

Tenant Administrators can view an end user’s details by navigating to _Manage Tenancy_ on the left-side menu of the SVX Portal, and selecting _End users_. Locate the end user in the list and select the horizontal ellipsis icon ⋯ alongside the end user entry to reveal menu options. Select _View_. You will be presented with the following information:
* The invitation QR Code (pending invitations only)
* User DID
* Invitation code
* Data invitation generated
* Date registered
* Status

<p align="center">
<img align="center" src="/.gitbook/assets/TA_20_View_End_Users_details.gif" alt="How to view an end user's details tutorial video." width="80%">
</p>

## Remove an end user from a Tenancy

To remove an end user form a Tenancy navigate to _Manage Tenancy_ on the left-side menu of the SVX Portal, and select _End users_. Locate the end user in the list and select the horizontal ellipsis icon ⋯ alongside the end user entry to reveal menu options. Select _Cancel and remove_ and confirm the removal of the end user via the modal window. The end user will be removed from the Tenant and will no longer appear in the list of end users.

> **Note**
> When an end user is removed from a Tenancy their associated end user vault is deactivated. Because of this, any connections between the end user and Organisations are cancelled. Any credentials that have been issued to the end user will remain in their wallet.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_21_Remove_End_User.gif" alt="How to remove an end user from a Tenancy tutorial video." width="80%">
</p>

## Restore an end user to a Tenancy

If a Tenant Administrator provides an end user with a newly generated invitation QR Code within 30 days of the end user being removed from the Tenancy, the end user will have their end user vault reinstated and will be able to restore their connections with Organisations.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_22_Reconnect_with_End_User.gif" alt="How to restore an end user to a Tenancy tutorial video." width="80%">
</p>
