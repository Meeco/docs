# SVX 4.0.1 Release Notes

**Software Release Date:** 1 July 2026

**Summary:**

This is a bugfix release for the SVX Platform.

## Bug Fixes

- Application API keys: creating a new key with the same name as a previously deleted key no longer fails with a unique constraint violation.
- Dashboard: fixed admin session expiring without a refresh token due to the `offline_access` scope being silently dropped by the OIDC provider when `prompt=consent` was absent.
- Fixed the `presentation_request.presentation_template` name, updated to `presentation_definition`. No API spec changes, only in code.
- Fixed Demo UI navigation after a credential offer is created.
- Fixed an issue where deploying multiple wallet configuration changes at the same time could fail.
