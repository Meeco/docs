## Add an Organisation to a Tenant

Tenant Administrators can add Organisations to their Tenancy by navigating to _Organisations_ on the left-side menu of the SVX Portal. Select the _Add new organisation_ button. You will be presented with a form consisting of the following fields:
* Organisation name
* Organisation URL
* Associated schemas (schemas made available to the Organisation)
* Logo upload

> **Note**
> _Associated schemas:_ For more information on creating, managing and assigning schemas to Organisations, see the tutorial Credential Schemas.

Once all required fields are complete select the _Add_ button and the Organisation will appear in the list.

> **Note**
> The Tenant Administrator who creates the Organisation will automatically become the Organisation's first Administrator. This allows you to invite other Organisation Administrators to join. It also provides you with access to the Organisation's context by clicking on the Organisation's name in the list.

> After entering the Organisation's context, you will be asked to set up the Organisation's [passphrase](/./concepts/terminology.md#passphrase). Meeco strongly advises that you do not set up the Organisation's [passphrase](/./concepts/terminology.md#passphrase). Instead, allow a known, invited member of the Organisation to set up the [passphrase](/./concepts/terminology.md#passphrase) when they first log in to the Portal. For more information, see the [Onboarding and Organisation Setup](../organisation-administrators/onboarding-and-organisation-setup.md) tutorial.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_06_Create_an_Organisation.gif" alt="How to add an Organisation to a Tenant tutorial video." width="80%">
</p>

## View and edit an Organisation’s details

To view the details of an Organisation within a Tenant, navigate to _Organisations_ on the left-side menu of the SVX Portal. Locate the Organisation in the list and select the horizontal ellipsis icon ⋯ alongside the Organisation’s name to reveal menu options. Select _View_. You will be presented with a _Details_ screen that lists the following information:
* Organisation Name
* Organisation URL
* Organisation ID
* Organisation DID
* Associated Schema(s)
* Logo

To edit the Organisation’s details, select the _Edit_ button from the _Details_ page or select _Edit_ from the horizontal ellipsis icon ⋯ alongside the Organisation’s name. The information will be presented as an editable form where you can update / change the following information:
* Organisation Name
* Organisation URL
* Associated Schema(s)
* Logo

When complete, select Update to save changes, or Cancel to discard changes.

> **Note**
> The Organisation’s _ID_ and _DID_ cannot be edited. These are system-generated identifiers that remain with the Organisation once it is created.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_23_View_and_edit_Organisation.gif" alt="How to view and edit an Organisation tutorial video." width="80%">
</p>

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

<p align="center">
<img align="center" src="/.gitbook/assets/TA_24_Archive_Organisation.gif" alt="How to archive an Organisation tutorial video." width="80%">
</p>

## Reinstate an Organisation

To reinstate an Organisation to a Tenant, navigate to _Organisations_ on the left-side menu of the SVX Portal. Locate the Organisation in the _Archived Organisations_ list and select the horizontal ellipsis icon ⋯ alongside the Organisation’s name to reveal menu options. Select _Reinstate_ and confirm the reinstating of the Organisation via the modal window. The Organisation will be reinstated to the Tenant, and will be moved to the _Current Administrators_ tab.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_25_Reinstate_Organisation.gif" alt="How to reinstate an Organisation tutorial video." width="80%">
</p>

## View an Organisation from the Organisation's context

To view an Organisation within the Organisation's context you must first be an Administrator of the Organisation. While Tenant Administators can add themselves as Organisation Administrators, without an Organisation's [passphrase](/./concepts/terminology.md#passphrase) you will have limited access to the Organisation and its functionality within the Portal. To obtain full access to the Organisation, you will need to enter the Organisation's [passphrase](/./concepts/terminology.md#passphrase).

> **Note**
> Meeco strongly advises that you first obtain consent from the Organisation to be added as an Administrator. After obtaining the Organisation's passphrase, you will be able to undertake actions on behalf of the Organisation.

For more information, see the following tutorials: [Manage Organisation Administrators](manage-organisation-administrators.md) and [Onboarding and Organisation Setup](../organisation-administrators/onboarding-and-organisation-setup.md).
