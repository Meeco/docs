# SVX 1.1.0 Release Notes

**Software Release Date**: May 17, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Verify Credentials**: Default credential schemas and better auditability for credentials
* **Portal enhancements**: Default credential schemas and better auditability for credentials
* **Identity Wallet enhancements**: Revamped branding & bugfixes
* **API Reference**: Published full API reference of the API is available [here](https://api-reference-sandbox.svx.exchange)
* **Stability**: In this release we focussed on improving the overall stability.

## New Features

### Verify Credentials

Added the option to verifying a credential next to the presentation. Functionality is used in the Meeco Wallet to allow holders to retrieve the status of a credential.

## Enhancements

### Meeco Wallet

In release 1.1.0 we have revamped the branding of the Identity Wallet, providing it with a fresh and modern look. With a refined visual identity, the Wallet application now offers a more visually appealing and cohesive interface, enhancing usability and ensuring a consistent brand experience across our platform.

Improvements:
* It is now possible to add multiple providers (i.e. tenanta) and between providers
* Persistent shares for credentials issued via vault
* App and Play Store listings are updated
* Logo for providers is now visible
* Wallet uses the credential verification endpoint
* Updated field mappings in various screens

### Portal

In this release, we have included a range of example credential schemas to provide a starting point for users. These pre-defined schemas serve as templates that users can leverage to create their own custom credential structures.

We have added a new feature that allows issuing organisation to view the recipients of a shared credential, providing transparency and control over data sharing. This significantly enhances auditability for those users and organisations without compromising security and privacy.

Improvements:
* No longer possible to use image urls for logos to avoid hotlinking issues
* Credentials issued use the issuer object notation which includes the organisation name as `issuer.name`
* It is now possible to edit the name of an application under organisation
* Easier to switch context as a user when you have multiple roles (e.g. tenant admin and organisation admin)

### Various

* Credential verification capability, alongside existing presentation verification.
* Verification results are also expanded and return more detailed information on exactly why the verification failed instead of a binary result.

## Deprecations and EOL

* Remove invitation code input from "Provider configuration" screen. Scanning or deeplinking is the only way to join a tenant as a wallet.