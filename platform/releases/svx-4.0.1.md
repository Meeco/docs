# SVX 4.0.1 Release Notes

**Software Release Date:** 1 July 2026

**Summary:**

This is a bugfix release covering the SVX Wallet and SVX Utilities components.

## SVX Wallet

### Bug Fixes

- Application API keys: creating a new key with the same name as a previously deleted key no longer fails with a unique constraint violation.
- Dashboard: fixed admin session expiring without a refresh token due to the `offline_access` scope being silently dropped by the OIDC provider when `prompt=consent` was absent.
- Fixed the `presentation_request.presentation_template` name, updated to `presentation_definition`. No API spec changes, only in code.
- Fixed Demo UI navigation after a credential offer is created.

### Other Changes

- Updated `@meeco/sd-jwt` to version `1.2.3` and `@meeco/sd-jwt-vc` to version `2.2.2`. Now uses package-provided reserved claim constants for `dc+sd-jwt` handling in the API and dashboard.

## SVX Utilities

### Bug Fixes

- Fixed handling of git conflicts during deployment: both rebase conflicts and push rejections are now detected. 
- Git operations are now serialized via a simple GenServer queue.
