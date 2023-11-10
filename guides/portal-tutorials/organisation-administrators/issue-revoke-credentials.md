## Issue a credential via a Vault connection

An Organisation Administrator can issue a credential to a vault connection by navigating to _Credentials_ on the left-side menu of the SVX Portal, and selecting _Issue / Revoke Credentials_. After selecting the _Issue credential_ button, you will be presented with two issuance methods. It is here you will select the radio button next to _Select connection_. A dropdown field will appear. Here you will need to select a connection from the list, then select the _Next_ button.

> **Note**
> If you have not yet created a connection with the intended Holder you will need to do this first. Navigate to _Enterprise Vault_ on the left-side menu and select _Connections_.

You will then need to select a credential template from a subsequent dropdown field. Select the credential template (this will form the basis of the verifiable credential you want to issue). You also have the option to select a _checkbox_ to enable revocation of the verifiable credential being issued. Once the required information is filled in, select the _Next_ button.

You will be presented with a form that displays all the attributes required from the Holder. Once you have filled in all required fields, select the _Issue_ button.

> **Note**
> The attributes listed in the form are defined by the schema used in the selected credential template.

On successful issuance of the verifiable credential, the issued credential will appear in a list with the status _Issued_.

<div align="center">
  <img src="/.gitbook/assets/OA_16_Issue_credential_via_connection.gif" alt="How to issue a credential via a Vault connection tutorial video." width="80%">
</div>

## View a credential

To view a credential navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _Issue / Revoke Credentials_. Find the credential in the list either by using the sort or search function. Once located, select the horizontal ellipsis icon ⋯ to reveal menu options. Select _View credential_.

You will be presented with a summary of the credential, including:
* Credential QR Code
* VC ID
* Credential template
* Date issued
* Expiry date
* Status
* Credential details
* The connection the credential has been issued to

<div align="center">
  <img src="/.gitbook/assets/OA_17_View_issued_credential.gif" alt="How to view a credential tutorial video." width="80%">
</div>

## Revoke a credential

To revoke a credential, navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _Issue / Revoke Credentials_. Find the credential in the list either by using the sort or search function. Once located, select the horizontal ellipsis icon ⋯ to reveal menu options. Select _Revoke credential_.

You will be presented with a confirmation message where you can choose to _Cancel_ or _Revoke_. If you decide to revoke the credential, you will return to the list of credentials where a confirmation toast message will appear. The revoked credential will no longer be visible in the list of credentials.

<div align="center">
  <img src="/.gitbook/assets/OA_18_Revoke_Credential.gif" alt="How to revoke a credential tutorial video." width="80%">
</div>
