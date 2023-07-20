## Introduction

Credential schemas define the contents and structure of a set of claims in a verifiable credential. Within the SVX Portal they are used by Organisation Administrators when issuing and verifying credentials. See Issue / Revoke Credentials and Verification Templates for more information. It is the responsibility of the Tenant Administrator to create and manage credential schemas, and assign them to Organisations within their Tenancy.

## Create a credential schema

Tenant Administrators can create new credential schemas by navigating to _Credentials_ on the left-side menu of the SVX Portal, and selecting _Credential schemas_. After selecting the _Create credential schema_ button, you will be presented with a form. The form consists of the following fields:
* Schema name
* Schema template
  * The option to select from a predefined list of schemas, including: Education Certificate, Student ID, Verifiable ID, and
  * The option to upload a template (this must be a JSON file)
* Available to

> **Note**
> _Uploaded schema template:_ The SVX Portal will check the uploaded schema to ensure it satisfies the predetermined file structure. If the schema does not comply, the system will advise the user via a notification message. The file must be uploaded as a JSON file.

> **Note**
> The _Available to_ field allows you to make schemas available to Organisations within your Tenancy. You can always add or remove Organisations.

When a predefined schema template has been selected, an editing panel will be displayed. Tenant Administrators can edit and customise these schema templates.

Once all required fields are complete select the _Create_ button and the credential schema will appear in a list with a designated Schema ID and Version Number.

## View and edit a credential schema

Tenant Administrators can view credential schemas by navigating to _Credentials_ on the left-side menu of the SVX Portal, and selecting _Credential schemas_. 

Locate the credential schema in the list and select the horizontal ellipsis icon ⋯ alongside the schema’s name to reveal menu options. Select _View_. You will be presented with the following information:
* Schema Name
* Schema ID
* Version
* Date Authored
* Schema details
* Available to

> **Note**
> The _Available to_ field allows you to make schemas available to Organisations within your Tenancy. You can always add or remove Organisations.

To edit the credential schema, select the _Edit_ button from the _View credential schema_ page or select _Edit_ from the horizontal ellipsis icon ⋯ alongside the schema’s name. The information will be presented as an editable form where you can update / change the following information:
* Available to

When complete, select _Save_ to save changes, or _Cancel_ to discard changes.

## Archive a credential schema

To archive a credential schema navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _Credential schemas_. Locate the credential schema in the _Active schemas_ tab and select the horizontal ellipsis icon ⋯ alongside the schema’s name to reveal menu options. Select _Archive_ and confirm the archiving of the credential schema via the modal window. The credential schema will be removed from the Tenant, and will be moved to the _Archived schemas_ tab.

> **Note**
> _Archiving a credential schema:_ Once archived, a credential schema will not be available to Organisations when creating credential and verification templates
> Note that credential schemas can be reinstated if required and full functionality will be restored to Organisations.

> **Note**
> Schemas are not permanently deleted from the system. Once published, it remains accessible through a public link. Credentials that have been issued that reference the schema can still be used.

## Reinstate a credential schema

To reinstate a credential schema navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _Credential schemas_. Locate the credential schema in the _Archived schemas_ tab and select the horizontal ellipsis icon ⋯ alongside the schema’s name to reveal menu options. Select _Reinstate_ and confirm the reinstating of the credential schema via the modal window. The credential schema will be reinstated to the Tenant, and will be moved to the _Active schemas_ tab.

