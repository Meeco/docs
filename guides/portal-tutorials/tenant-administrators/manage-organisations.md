## Add an Organisation to a Tenant

Tenant Administrators can add Organisations to their Tenancy by navigating to _Organisations_ on the left-side menu of the SVX Portal. Select the _Add new organisation_ button. You will be presented with a form consisting of the following fields:
* Organisation name
* Associated schemas (schemas made available to the Organisation)
* Logo upload

> **Note**
> _Associated schemas:_ For more information on creating, managing and assigning schemas to Organisations, see the tutorial Credential Schemas.

Once all required fields are complete select the _Add_ button and the Organisation will appear in the list.

![How to add an Organisation to a Tenant tutorial video.](/.gitbook/assets/06_TA_Create_an_Organisation.gif)

## View and edit an Organisation’s details

To view the details of an Organisation within a Tenant, navigate to _Organisations_ on the left-side menu of the SVX Portal. Locate the Organisation in the list and select the horizontal ellipsis icon ⋯ alongside the Organisation’s name to reveal menu options. Select _View_. You will be presented with a _Details_ screen that lists the following information:
* Organisation Name
* Organisation ID
* Organisation DID
* Associated Schema(s)
* Logo

To edit the Organisation’s details, select the _Edit_ button from the _Details_ page or select _Edit_ from the horizontal ellipsis icon ⋯ alongside the Organisation’s name. The information will be presented as an editable form where you can update / change the following information:
* Organisation Name
* Associated Schema(s)
* Logo

When complete, select Update to save changes, or Cancel to discard changes.

> **Note**
> The Organisation’s _ID_ and _DID_ cannot be edited. These are system-generated identifiers that remain with the Organisation once it is created.

## Archive an Organisation

To archive an Organisation within a Tenant, navigate to _Organisations_ on the left-side menu of the SVX Portal. Locate the Organisation in the _Current Organisations_ list and select the horizontal ellipsis icon ⋯ alongside the Organisation’s name to reveal menu options. Select _Archive_ and confirm the archiving of the Organisation via the modal window. The Organisation will be removed from the Tenant, and will be moved to the _Removed Administrators_ tab.

> **Note**
> _Archiving an Organisation:_ Once removed from a Tenant, all associated Organisation Administrators will no longer be able to access the Organisation or its functions, this includes the management of:
> * Credential Templates
> * Credential issuance and revocation
> * Verification Templates
> * Request for Credentials
> * Connection with End Users
> * Applications
> Note that Organisations can be reinstated if required and full functionality will be restored.

## Reinstate an Organisation

To reinstate an Organisation to a Tenant, navigate to _Organisations_ on the left-side menu of the SVX Portal. Locate the Organisation in the _Archived Organisations_ list and select the horizontal ellipsis icon ⋯ alongside the Organisation’s name to reveal menu options. Select _Reinstate_ and confirm the reinstating of the Organisation via the modal window. The Organisation will be reinstated to the Tenant, and will be moved to the _Current Administrators_ tab.
