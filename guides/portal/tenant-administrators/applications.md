## Introduction

Applications leverage [OAuth2's client credentials grant](https://oauth.net/2/grant-types/client-credentials/), a protocol designed for machine-to-machine (M2M) authentication. In this setup, a client ID and client secret are used to authenticate and obtain an access token. This token, representing the specific application rather than a user, is then used to authorise the application to perform specific operations such as creating credential schemas, managing end-users, and tenant administrators within the context of a particular Tenant. This approach enables secure and controlled access for services.

## Create an application

Tenant Administrators can create a new application by navigating to Devtools on the left-side menu of the SVX Portal, and selecting Applications. After selecting the Create application button, you will be presented with a form. The form consists of the following fields:
* Application name
* Application description

> **Note**
> _Application description:_ Enter a description so that users know what the application is and / or is used for.

Once all required fields are complete select the Create application button and the application will appear in a list with a designated Client ID.

## View and edit an application

Tenant Administrators can view applications by navigating to Devtools on the left-side menu of the SVX Portal, and selecting Applications.

Locate the application in the list and select the horizontal ellipsis icon ⋯ alongside the application’s name to reveal menu options. Select View. You will be presented with the following information:
* Application name
* Application description
* Domain
* Client ID
* Client secret

> **Note**
> _Client ID:_ You can copy this information to your clipboard.

> **Note**
> _Client secret:_ This field is hidden for security and privacy reasons. Click on the eye 👁 icon to reveal the entire secret.

To edit the application’s details, select the Edit button from the View application page or select Edit from the horizontal ellipsis icon ⋯ alongside the application’s name. The information will be presented as an editable form where you can update / change the following fields:
* Application name
* Application description

When complete, select Save to save changes, or Cancel to discard changes.

## Enable machine-2-machine authentication

A Tenant Administrator can provide an application with authenticated access to SVX functionality. This allows the application to perform the following actions:
* Invite and remove end-users
* Manage credential schemas
* Create, archive and reinstate Organisations
* Invite, remove and reinstate Tenant Administrators

Once an application has been created, the Tenant Administrator can navigate to Devtools on the left-side menu of the SVX Portal, select Applications, locate the application in the list and select the horizontal ellipsis icon ⋯ alongside the application’s name to reveal menu options. After selecting View you will be able to copy the Client ID and Client Secret which you can use to provide your application authenticated access to the SVX Platform.

## Delete an application

To remove an application from your Tenant navigate to Devtools on the left-side menu of the SVX Portal, and select Applications. Locate the application in the list and select the horizontal ellipsis icon ⋯ alongside the application’s name to reveal menu options. Select Delete and confirm the deletion of the application via the modal window. The application will be permanently deleted from the Tenant, and will no longer appear in the list.
