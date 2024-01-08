# SVX 1.3.0 Release Notes

**Software Release Date**: Jan , 2024

**Summary**:

The following is a summary of the key features and enhancements:

* **New functionalities**: Implemented the ability to manage Organisation Website URL in Organisation Details for all administartors. Added the ability to establish connection between Organisation and Meeco Wallet Holders via QR-code. It's possible now to change password in Manage Account page. Also now Organisation Administartor can decide to include ot not the logo for the credential template.
* **Enhancements**: Added "logout" button if the user lost his access to SVX Portal during the active session. Improved usability by adding a QR-code for "View pending connections details", so that no necessity to create a new invitation if previously generated QR-code hasn't been used for some reasons. Improved work of search and pagination for page "Connections. Pending invitations" by switching to backend-based pagination. login page design bacame for user friendly on mobile browser.
* **Security updates**: After security audit was performed the procedures for fixing and improvements all important security issues on frontend and backend sides.
* **Bug Fixes**: Various bug fixes for the portal, including fixes for spelling errors, pagination and search issues, and improvements to the error message displayed when an incorrect email address is entered when inviting an admin.


## Enhancements

### Portal

* Added a pagination feature for End Users pages.
* Implemented a new loading screen.
* Updated notification email footer text and logo.
* Updated text size and wording in Password Reset notification emails.
* Added the option "Delete" for the verification response.


## Bug Fixes

This release also includes bug fixes aimed at improving the user experience.

### Portal

* Fixed display of search results and pagination during the search process or its reset.
* Fixed the issue with "Bad request exception" during the invitation of a new admin to the Organisation.
* Fixed pagination issue in Credential Templates, Verification Templates and Verification requests pages: pagination should be removed when less than 10 results during some removing records process.
* Fixed spelling issue: The word "credential" is misspelt in the description text (sub-heading).
* The tooltip link now opens in a new browser tab.
* Fixed issue with changing the search field icon to a cross when text has been entered.


## Various

* Our own custom implementation of the controllers/actions for loading/registering vault keys and organisation DID have been replaced with calls to @meeco/sdk so that now the same format is used in all our services.

## Deprecations and EOL

None
