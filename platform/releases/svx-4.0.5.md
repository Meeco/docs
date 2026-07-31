# SVX 4.0.5 Release Notes

**Software Release Date:** 31 July 2026

**Summary:**

This is a bugfix release for the SVX Platform.

## Improvements

- Added support for RS256 signing keys, in addition to the existing algorithm, for both locally managed keys and keys managed via AWS KMS.

## Bug Fixes

- Fixed an issue where credential presentations could report inconsistent holder binding information depending on the credential format, which could cause presentation requests with strict requirements to fail unexpectedly.
- Fixed a database compatibility issue that could prevent the Wallet from running on PostgreSQL 16 and newer.
