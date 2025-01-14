# SVX 1.2.3 Release Notes

**Software Release Date**: Oct 05, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Wallet**: Published version 1.7.0 into Stores. The new version allows us to solve the issue of establishing the connection between Wallet and Organisation.
* **Enhancements**: Made the generation of invitation codes easier by removing the ambiguity of characters
* **Bug Fixes**: This release addresses numerous bugs as part of our quest to provide a quality product. Most of the bugs are related to updating toast messages or text in modal windows. The text of emails has been changed depending on the Administrator role during the archive tenant.


## Enhancements

### Vault

We have made the invitation token for establishing the connection between the organisation and the user easier by removing ambiguous characters like 0 (number) and O (letter). This should make it easiers to copy the token.


## Bug Fixes

This release also includes bug fixes aimed at improving the user experience.

### Portal

* Updated order of fields in the administrator's details page.
* Fixed spelling errors in modal messages during reinstating or inviting administrators, and managing applications.
* Fixed expand/collapse action for "Profile menu" depending on the user's navigation within the Portal menu.
* Fixed email notification for Tenant and Organisation Administrator when a Tenant has been archived.
* Fixed user redirection to the previous screen, if he cancels the creation of a new Credential Template or new Connection during the issuance process.

### Wallet

* Fixed an issue with establishing a connection between Meeco Wallet End-User and Organisation.

## Deprecations and EOL

None
