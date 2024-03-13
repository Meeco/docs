# SVX 1.3.1 Release Notes

**Software Release Date**: March 04, 2024

**Summary**:

This release introduces several enhancements and bug fixes related to the SVX Portal and Meeco Wallet

* **Enhancements**: Introduced a new approach for displaying encrypted data for organization administrators depending on the entered passphrase. Added a Toolbar with links to Documentation and HelpDesk. Updated the empty screens "Credentials" and "Requests" with animations for Meeco Wallet.
* **Bug Fixes**: Resolved a bug causing a 500 error during the opening/managing of verification responses. Fixed the ability to open a Tenant or Organisation right after their creation. Various bug fixes for the Wallet application mostly related to User Experience improvements.
  
## Enhancements

### Portal

* A new toolbar has been added to the Portal enabling users to view and access: Security-related information, SVX Documentation and SVX Helpdesk.
* Updated the screen View credential based on security aspects and whether a passphrase was entered or not.

  
### Wallet

* Updated the margins on the Processing and Registering screens.
* Updated the Credentials empty state screen.
* Added animations on empty state screens: Credentials and Requests.
* Updated "Secure Your Wallet" and "Biometrics" screens.

  
## Bug Fixes

This release also includes bug fixes aimed at improving the user experience.


### Portal

* Resolved an issue with the 500 error during managing verification responses.
* Fixed a bug where the error message appears for credentials with a different format than vc-jwt in View mode.
* Renamed error message for already existing applications with the same name.
* Provided the ability to open Tenant and Organization right after their creation from the lists.
* Fixed white text appearing on the credentials card in the credentials template.

### Wallet

* Icons and main image are displayed now on the "Settings" ->  "Terms&Conditions" screen.


## Deprecations and EOL

None
