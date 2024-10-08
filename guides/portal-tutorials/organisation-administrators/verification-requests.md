## Create a verification request

In order to request credentials for verification from Holders, an Organisation Administrator must first create a verification request. An Organisation Administrator can create a new verification request by navigating to Credentials on the left-side menu of the SVX Portal, and selecting _Verification requests_.

After selecting the _Create verification request_ button, you will be presented with a form. The form consists of the following fields:
* Request name
* Verification template

Once these fields have been filled in, select _Create_. You will be presented with an _Request QR Code_ which contains the request for credentials. Present the QR Code to a Holder either by downloading and sending the QR Code directly to them, or show the QR Code to the Holder in-person.

Below the QR Code is a summary of the request details, including:
* Request ID
* Request name
* Verification template
* Date created on
* Number of responses

Below the request details is a table that displays the received request responses. This table will be populated as Holders scan the QR Code and submit their verifiable credentials for verification.

<div align="center">
  <img src="/.gitbook/assets/OA_23_Create_Verification_Request.gif" alt="How to create a verification request tutorial video." width="80%">
</div>

## View a verification request

Organisation Administrators can view a verification request by navigating to _Credentials_ on the left-side menu of the SVX Portal, and selecting _Verification requests_. Locate the verification request in the list and select the horizontal ellipsis icon ⋯ alongside the request’s name to reveal menu options. Select _View_. You will be presented with the following information:
* Request QR Code
* Request details
* Request responses

<div align="center">
  <img src="/.gitbook/assets/OA_24_View_Verification_Request.gif" alt="How to view a verification request tutorial video." width="80%">
</div>

## Archive a verification request

To archive a verification request, navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _Verification requests_. Locate the verification request in the list. Select the horizontal ellipsis icon ⋯ alongside the request’s name to reveal menu options. Select _Archive_ and confirm the archiving of the verification request via the modal window. The verification request will be removed from the Organisation.

> **Note**
> _Archiving a verification request:_ All previous responses received for the archived verification request will also be archived.

<div align="center">
  <img src="/.gitbook/assets/OA_25_Archive_Verification_Request.gif" alt="How to archive a verification request tutorial video." width="80%">
</div>

## Restore a verification request

To restore a verification template navigate to Credentials on the left-side menu of the SVX Portal, and select Verification templates. Locate the verification template in the Archived templates tab and select the horizontal ellipsis icon ⋯ alongside the template’s name to reveal menu options. Select Restore and confirm the reinstating of the verification template via the modal window. The verification template will be restored to the Organisation, and will be moved to the Active templates tab.

<div align="center">
  <img src="/.gitbook/assets/OA_26_Restore_Verification_Request.gif" alt="How to restore a verification request tutorial video." width="80%">
</div>

## View a request response

As Holders scan the QR Code and submit their verifiable credentials for verification, their responses will be available for viewing. To view a request response, navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _Verification requests_. Locate the verification request in the list that the response relates to. Select the horizontal ellipsis icon ⋯ alongside the request’s name to reveal menu options. Select _View_. Scroll to the bottom of the page where you will find a table that lists all associated request responses.

Responses can return one of three statuses:
* Pending (waiting for a Holder to submit credentials).
* Verified (the submitted credentials have been successfully verified and meet the request requirements).
* Failed (indicates that the submitted credentials have been revoked or expired).
  * A failed response will include details of what, specifically, failed to verify. 

For further details, locate the response in the table and select the horizontal ellipsis icon ⋯ alongside to reveal menu options. Select _View_. You will be presented with the following information:
* Submission ID
* Wallet DID
* Date received
* Status
* Number of credentials returned
  * Details of each credential returned

<div align="center">
  <img src="/.gitbook/assets/OA_27_View_Verification_Response.gif" alt="How to view a verification response tutorial video." width="80%">
</div>

## Verify a credential

Once a Holder has submitted credentials via a verification request, an Organisation Administrator can verify the returned credentials. Follow the steps mentioned above to _view a request response_. On the _view response_ screen, select the _Verify credential(s)_ button.

The status of the response will change from _Pending_ to either _Valid_ or _Expired / Revoked_.

> **Note**
> _Verifying credentials:_ A credential’s status is accurate at the time of verification. Credential verification does not happen automatically. To ensure accurate credential status, you will need to re-verify credentials as often as required.

<div align="center">
  <img src="/.gitbook/assets/OA_29_Verify_credential.gif" alt="How to verify a credential tutorial video." width="80%">
</div>
