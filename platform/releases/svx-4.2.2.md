# SVX 4.2.2 Release Notes

**Software Release Date:** 28 August 2026

**Summary:**

This is a maintenance and security release for SVX.

## Security

- A broad round of dependency upgrades across the platform to close known vulnerabilities flagged by routine security scanning.

## Changed

- Upgraded the core web framework (NestJS) from 10.4.1 to 11.2.1, which brings in Express 5 under the hood.
- Upgraded the OpenID Connect provider library from 9.7.0 to 9.11.3.

## Bug Fixes

- Fixed an issue where SVX Verify could send the user back to the identity provider a second time even though their presentation had already succeeded. The share page now checks whether the session is already done before it tries to launch the wallet again, so a completed verification stays completed.
- Fixed API error responses for failed OAuth logins coming back empty. When an identity provider rejected a login attempt, integrators previously had no way to tell why. The response now includes the actual error and description reported by the identity provider, so the real cause is visible instead of a blank result.
