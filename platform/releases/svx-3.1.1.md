# SVX 3.1.1 Release Notes

**Software Release Date:** 26 November 2025

**Summary:**

This release includes fixes related to SVX Verify cookie issues.

## Organisation Wallet

### SVX Verify

- Allowed the svx_verify_session cookie to persist on the success screen in SVX Verify, enabling language changes within the page.
- Removed session verification for the GET endpoint so that the presentation request can be accessed by external wallets
- Fixed issue where the terms-checkbox was not throwing an error after session expiry.
- Fixed issue where the `terms-checkbox` was not throwing an error after session expiry.
