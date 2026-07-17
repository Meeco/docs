# SVX 4.0.3 Release Notes

**Software Release Date:** 17 July 2026

**Summary:**

This is a bugfix release for the SVX Platform.

## Bug Fixes

- Fixed an issue where errors from an external identity provider during a Bridge login were not handled gracefully, leaving the interaction in an inconsistent state instead of cleanly aborting it.
- Fixed an issue where credential offers could be generated incorrectly in some issuance flows.
- Fixed a mismatch between the API documentation and actual behavior for retrieving a wallet's credentials.
- Fixed verification of digital credential responses to correctly check who the response was intended for.
- Fixed an issue where display information from the issuer was sometimes not captured correctly when receiving a credential.
- Corrected the API documentation for receiving a credential to match what the endpoint actually expects.
- Fixed an encoding issue affecting mobile driving license (mDL) credential issuance.

## Security

- Pinned a fixed, known user ID for the non-root user in a few container images, so deployments can enforce a specific user via Helm security settings.
