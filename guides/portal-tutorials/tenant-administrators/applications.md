## Introduction

Applications leverage [OAuth2's client credentials grant](https://oauth.net/2/grant-types/client-credentials/), a protocol designed for machine-to-machine (M2M) authentication. In this setup, a client ID and client secret are used to authenticate and obtain an access token. This token, representing the specific application rather than a user, is then used to authorise the application to perform specific operations such as creating credential schemas, managing end-users, and tenant administrators within the context of a particular Tenant. This approach enables secure and controlled access for services.

## Create an application

Tenant Administrators can create a new application by navigating to _Devtools_ on the left-side menu of the SVX Portal, and selecting _Applications_. After selecting the _Create application_ button, you will be presented with a form. The form consists of the following fields:
* Application name
* Application description

> **Note**
> _Application description:_ Enter a description so that users know what the application is and / or is used for.

Once all required fields are complete select the Create application button and the application will appear in a list with a designated Client ID.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_16_Create_new_application.gif" alt="How to create an application tutorial video." width="80%">
</p>

## View and edit an application

Tenant Administrators can view applications by navigating to _Devtools_ on the left-side menu of the SVX Portal, and selecting _Applications_.

Locate the application in the list and select the horizontal ellipsis icon ⋯ alongside the application’s name to reveal menu options. Select _View_. You will be presented with the following information:
* Application name
* Application description
* Domain
* Client ID
* Client secret

> **Note**
> _Client ID:_ You can copy this information to your clipboard.

> **Note**
> _Client secret:_ This field is hidden for security and privacy reasons. Click on the eye 👁 icon to reveal the entire secret.

To edit the application’s details, select the _Edit_ button from the _View application_ page or select Edit from the horizontal ellipsis icon ⋯ alongside the application’s name. The information will be presented as an editable form where you can update / change the following fields:
* Application name
* Application description

When complete, select _Save_ to save changes, or _Cancel_ to discard changes.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_17_View_and_edit_application.gif" alt="How to view and edit an application tutorial video." width="80%">
</p>

## Enable machine-2-machine authentication

A Tenant Administrator can provide an application with authenticated access to SVX functionality. This allows the application to perform the following actions:
* Invite and remove end-users
* Manage credential schemas

Once an application has been created, the Tenant Administrator can navigate to _Devtools_ on the left-side menu of the SVX Portal, select _Applications_, locate the application in the list and select the horizontal ellipsis icon ⋯ alongside the application’s name to reveal menu options. After selecting _View_ you will be able to copy the Client ID and Client Secret which you can use to provide your application authenticated access to the SVX Platform.

For more information, see the [Machine-2-Machine Communication](/guides/api-guides/machine-2-machine-communication.md) guide.

## Delete an application

To remove an application from your Tenant navigate to _Devtools_ on the left-side menu of the SVX Portal, and select _Applications_. Locate the application in the list and select the horizontal ellipsis icon ⋯ alongside the application’s name to reveal menu options. Select _Delete_ and confirm the deletion of the application via the modal window. The application will be permanently deleted from the Tenant, and will no longer appear in the list.

<p align="center">
<img align="center" src="/.gitbook/assets/TA_18_Delete_application.gif" alt="How to delete an application tutorial video." width="80%">
</p>
