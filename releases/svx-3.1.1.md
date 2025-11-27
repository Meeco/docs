# SVX 3.1.1 Release Notes

**Software Release Date:** 26 November 2025

**Summary:**
Summary:
This release includes fixes related to SVX Verify cookie issues and improves the user experience in our issuance and verification testing tools.Component Updates

# Component Updates
## Organisation Wallet
### Fixed
- Allowed the svx_verify_session cookie to persist on the success screen in SVX Verify, enabling language changes within the page.
- Removed session verification for the GET endpoint so that the presentation request can be accessed by external wallets
- Fixed issue where the terms-checkbox was not throwing an error after session expiry.

### Changed
- Updated the look and feel of the test pages at /test/issue and /test/verify (previously /test/present). No functional changes have been made, rather the pages have been updated to improve testing by showing more (useful) information. 
  - The following javascript libraries were added to /public/vendor:
    - Monaco editor
    - TailwindPlus Elements (for modal component and mobile menu)
- Renamed /public/vendor/index.umd.min.js to /public/vendor/jose.min.js to reflect the library name.