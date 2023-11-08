## Introduction

Connections can be established between Organisations and end-users to facilitate secure, end-to-end encrypted sharing of data. An Organisation within the Portal is provided with an [Enterprise Vault](/platform/vault/enterprise-vault.md) and end-users who have registered with the Organisation’s associated Tenant, are provided with an [end-user Vault](/platform/vault/README.md). A connection between an Organisation and an end-user allows Organisations to directly issue, revoke and verify an end-user’s credentials.

## Invite a user or an Organisation to connect

Organisation Administrators can invite end-users and other Organisations to connect by navigating to _Enterprise Vault_ on the left-side menu of the SVX Portal, and selecting _Connections_. After selecting the _Generate invitation code_ button, you will be presented with a field titled _Recipient identifier_. Enter an identifying label in the field.

> **Note**
> _Recipient identifier:_ Enter a name, email address, unique number etc. to identify the connection.

Once complete select the _Generate invitation code_ button. You will be provided with an invitation code which you will need to send to the connection recipient. You may wish to send the invitation code via email, direct message, or share it in-person.

Return to the _Connections_ list and you will see the connection appear in the _Pending connections_ tab.

> **Note**
> Once the connection recipient has accepted the invitation, the connection will move to the _Existing connections_ tab.

> <p align="center">
<img align="center" src="/.gitbook/assets/OA_12_Generate_connection_code.gif" alt="How to invite a user or Organisation to connect tutorial video." width="80%">
</p>

## Accept an invitation to connect

Organisation Administrators can accept an invitation to connect from other Organisations by navigating to _Enterprise Vault_ on the left-side menu of the SVX Portal, and selecting _Connections_. After selecting the _Accept an invitation_ button, you will be presented with a form. The form consists of the following fields:
* Recipient identifier
* Invitation code

> **Note**
> _Recipient identifier:_ Enter a name, email address, unique number etc. to identify the connection.

> **Note**
> _Invitation code:_ The invitation code will need to be provided by the Organisation requesting to connect. The code may be provided be any means of communication, eg. email, direct message, or in-person.

Once complete, select the _Connect_ button. You will be presented with a confirmation modal that asks you to agree to:
* Sharing your Organisation’s name and DID
* Enabling the sharing of items between your Organisation and this user.

Select the _Confirm_ button to complete the connection. The new connection will appear in the list of _Existing connections_. Or, select the _Cancel_ button to reject the connection.

> <p align="center">
<img align="center" src="/.gitbook/assets/OA_33_ Accept_a_connection_invitation.gif" alt="How to accept an invitation to connect tutorial video." width="80%">
</p>

## View a connection

Connections can exist in two states:

* Pending, and
* Existing

To view a _Pending_ connection navigate to _Enterprise Vault_ on the left-side menu of the SVX Portal, and select _Connections_. Select the _Pending connections_ tab, locate the connection in the list and select the horizontal ellipsis icon ⋯ alongside the connection’s name to reveal menu options. Select _View_. You will be presented with the following information:
* Placeholder name
* Connection ID
* Token
* Date created

> <p align="center">
<img align="center" src="/.gitbook/assets/OA_13_View_pending_connection.gif" alt="How to view a pending connection tutorial video." width="80%">
</p>

To view an _Existing_ connection navigate to _Enterprise Vault_ on the left-side menu of the SVX Portal, and select _Connections_. Select the _Existing connections_ tab, locate the connection in the list and select the horizontal ellipsis icon ⋯ alongside the connection’s name to reveal menu options. Select _View_. You will be presented with the following information:
* Recipient identifier
* Connection ID
* Date created
* Items that are currently being shared

> **Note**
> _Items being shared:_ This includes credentials that have been issued via a Vault-to-Vault share.

> <p align="center">
<img align="center" src="/.gitbook/assets/OA_14_View_existing_connection.gif" alt="How to view an existing connection tutorial video." width="80%">
</p>

## Cancel and remove a connection

To cancel and remove a connection navigate to _Enterprise Vault_ on the left-side menu of the SVX Portal, and select _Connections_. Locate the connection in either the _Existing connections_ or _Pending connections_ tab and select the horizontal ellipsis icon ⋯ alongside the connection’s name to reveal menu options. Select _Cancel and remove_ and confirm the cancelation of the connection and the removal of the connection from your Organisation via the modal window. The connection will be removed from the list of connections.

> **Note**
> To re-establish a connection, you will need to generate a new invitation code, or accept an invitation with the end-user or Organisation once again.

> **Note**
> When an end-user is removed as a connection, they will be notified in their wallet application that the connection has been cancelled. All issued credentials will remain in their wallet.
> 

> <p align="center">
<img align="center" src="/.gitbook/assets/OA_15_Cancel_and_remove_connection.gif" alt="How to cancel and remove a connection tutorial video." width="80%">
</p>
