# SVX 1.2.4 Release Notes

**Software Release Date**: Nov 7, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Enhancements**: Improved the context-switching menu options in the Portal based on the access levels of the administrator
* **Consistent display of archived Credential Schema**: Archived credential schemas are displayed in the lists with the appropriate mark "archived" but no ability to use/select the archived objects for Credential Templates, Verification Templates, or organisation details.
* **Bug Fixes**: Various bug fixes in Portal including corrections to spelling errors, resolution of UI glitches that occur when inviting administrators, and improvements to the cancellation of invitations for both administrators and end-users.


## Enhancements

### Portal

* Updated context-switching options in Portal. "Return to Global Dashboard", "Return to Tenant Dashboard" and "Switch Tenant or Organisation" options only show for administrators with multiple levels of access and are hidden for other administrators.
* Changed the navigation to the previously selected option to display rows on the page when the user clicks "Back" in the browser after opening a page from the list.
* Updated all instances where "Meeco OIDC" appears as the page title to "Meeco SVX Identity Provider".
* Updated UI for the explanation of verification response status.
* Archived Credential Schema now appears in the "Associated to" field on an Organsiation's details page.
* Archived Credential Schema(s) appear as 'archived' and disabled when creating a Verification Template and Credential Template.
* Updated pagination wdiget to add the ability to select the number of pages per row and the ability to navigate to the desired page number by entering the page.
* An archived administrator (not associated with any tenant or organisation) is now automatically logged out of the system.
* Removed the necessity to select credential template during issuance flow if administrator does it via Credential Template -> Issue Credential.
* Changed the UX when removing an 'Expired' end user QR Code registration invitation.

## Bug Fixes

This release also includes bug fixes aimed at improving the user experience.

### Portal

* Fixed the ability to verify the revoked credential.
* Fixed issue with cleaning up the list with pending admins during cancellation invitations.
* Fixed issue with the appearance of a wrong tooltip while inviting a new admin.
* Fixed displaying the organisation lists in the "Available to" field for the View Credential Schema page.
* Fixed issue with context switching screen for users who no longer have access to SVX OA Context.
* Fixed displaying the archived Credential Schema on an Organsiation's details page.
* Fixed administrator's navigation if he decides to remain logged in during the logout process.
* Fixed issue with the pagination if less than 10 records per page during some removing records process.
* Fixed issue with the changing organisation name.
* Fixed tooltip message when cancelling a new end-user registration QR Code.
* Fixed spacing between tab, sub-heading and table heading components, full stops at the end of sentences.
* Fixed name of uploaded image name of organisation logo.
* Fixed error messages for empty fields on the login screen.
* Fixed opening links to Privacy Policy and T&Cs from the logout and login page should open in new browser tabs.

## Deprecations and EOL

None
