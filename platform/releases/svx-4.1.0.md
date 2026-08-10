# SVX 4.1.0 Release Notes

**Software Release Date:** 7 August 2026

**Summary:**

This is a feature release for the SVX Platform, led by several new Wallet capabilities, alongside smaller improvements and fixes.

## New Features

- SVX Verify sessions can now report the specific cryptographic key a presentation was bound to, making it easier for integrators to confirm proof of possession.
- **Transaction Data support** (per the [OpenID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) specification): Verifiers can now attach additional context to a presentation request, and wallets can restrict which types of this data they are willing to accept. Verifiers can confirm that the credential presentation returned by a wallet was genuinely bound to the requested context, adding an extra layer of assurance to verification flows.
- Certificate management now shows which signing key a managed certificate belongs to, and certificate listings support filtering and pagination.
- Signing requests can now accept base64url-encoded input, in addition to plain text.
- Session data used by the authorization server can now be stored in Postgres as an alternative to Redis.
- OAuth clients can now be marked as native or web applications, enabling correct authentication behavior for native app integrations.


## Improvements

- Failed SVX Verify presentations now include a more detailed reason, making it easier to understand why a verification did not succeed.
- The demo issuance and verification pages have moved into the main dashboard; the previous standalone demo pages have been retired.
- SVX Verify no longer displays a country flag or country name for identity providers and wallets.
- Credential template colors must now be valid hex colors; unsupported formats such as gradients are no longer accepted.
- Updating a credential template's validity settings now fully replaces the previous settings, so clearing a value (such as removing an expiry) is applied correctly.
- The default signing algorithm used for admin and access tokens has changed from ES256 to RS256.
- Runtime configuration is no longer cached by the browser, so configuration changes take effect immediately.

## Bug Fixes

- Fixed an issue where some internal errors were silently discarded instead of being logged, making certain failures harder to diagnose.
- Fixed the SVX Verify PDF receipt being cut off instead of spanning multiple pages when a presentation contained a lot of information.
- Fixed claim labels in the PDF receipt so that untranslated claims show a readable name instead of a raw technical key.
- Fixed images, such as a portrait photo, not displaying correctly on the SVX Verify success page and in the PDF receipt.
- Fixed an issue where cancelling a verification session at the wrong moment could send the user to a generic error page instead of the expected cancellation page.
- Fixed the "Save & Preview" redirect URL in the SVX Verify configuration screens.
- Fixed an image format (JPEG2000) not rendering correctly in presentation responses.
- Fixed credential issuance failing for templates with validity dates configured.
- Improved reliability of database setup during deployment on managed Postgres providers (such as AWS Aurora) that restrict administrator permissions.
