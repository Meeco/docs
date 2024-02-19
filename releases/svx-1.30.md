# SVX 1.3.0 Release Notes

**Software Release Date**: Feb 27, 2024

**Summary**:

This release introduces several new functionalities, enhancements, bug fixes, and security updates aimed at improving the overall user experience and security of the system.

* **New functionalities**: Organisations can add a Website URL and users can change their user password directly from the Portal.
* **Enhancements**: User Experience has been optimised in terms of design, and usability of using pagination for the Connections page, introduced the ability for Organisation Administrators to use a generated QR code to establish a connection, and a new section "Documentation" has been added to "Devtools" block. The consistency in UI for the Wallet application has been enhanced as well.
* **Security updates**: Conducted a thorough security audit and addressed various security issues on both the frontend and backend
* **Bug Fixes**: Various bug fixes for the portal mostly related to User Experience improvements. Fixed multiple issues with navigation buttons, fixed a bug displaying incorrect status for the Revoked credentials in verification responses. Addressed some issues preventing the archiving of Organisation Administrators.


## New Functionality

### Portal 

* Organisation Administrators and Tenant Administrators can now view and edit the Organisation website URL for an Organisation..
* Added a QR code to "View pending connection details," streamlining the setup of a connection process.
* Administrators can now change their passwords directly from within the Portal.
* Organisation Administrators can choose whether to include a logo in the credential template.

  
## Enhancements

### Portal

* Optimized the login page design for improved user-friendliness on mobile browsers.
* Implemented backend-based pagination for the Connections page, pending connections, and Credential Template list.
* Introduced the ability for Organisation Administrators to use a generated QR code to establish a connection.
* Added the new sections "Documentation" and "Help Desk" into "Devtools" block.
* Provided the ability to log out from the page "User no longer has access".


## Bug Fixes

This release also includes bug fixes aimed at improving the user experience.

### IDP

* Fix `iss` and `aud` claims in id_token and access_token
* Add rate limiting for security

### Portal

* Resolved an issue where an atom delegation error occurred during the setup passphrase process.
* Fixed a bug where the verification response incorrectly showed "Verified" instead of "Revoked" after revocation.
* Addressed an issue preventing the archiving of organisation administrators.
* Fixed issues with the "Back to Login" button on the Reset Password page and resolved dropdown functionality problems on the SVX Platform.
* Resolved pagination problems when switching between tabs on the SVX Portal.
* Removed unused/non-functional icons from UI.
* Fixed the various bugs related to UI components and updated the hyperlinks to valid URLs.

### Wallet

 * Rectified differences in the Terms and Conditions and Privacy Policy UI between onboarding and Settings in the Wallet.


## Security Updates

* Conducted a security audit and addressed various security issues on both the frontend and backend.


## Deprecations and EOL

None
