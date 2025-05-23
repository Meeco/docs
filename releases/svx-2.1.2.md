# SVX 2.1.2 Release Notes

**Software Release Date:** 23 April 2025

**Summary:**

This update delivers a range of bug fixes and improements across multiple components, enhancements to configuration management, and security-focused upgrades of dependencies.

## Portal

### Added

- Added `Format`, `Issuer Id` and `Wallet DID` field on `View Credential screen`
- Added `Format` column to `View Credentials` screen table
- Added `Loading` animation to all list views and all pages
- Added additional `Credential Styling` options to `Create Credential Type/Edit Credential Type` to include:
  - Solid color
  - Pattern
  - Custom Image

### Changed

- Refactored the `globalAdmin` and `invitation` using RTK query
- Refactored the `application` using `redux-toolkit` query
- Refactored the `credentialIssuance` Controller and move the logic to api
- Renamed `credential template`, `credential definition` and `credential request` to be consistent with `meeco-vc`
- Changed description for the table on `View Credentials` screen
- Changed the check for `Revokable VCs` to also include a check for format = `jwt_vc_json`
- Changed `Request Name` to be show the `Template Name` instead of `ID` on `Verification requests` screen, also changed column position in the table
- Changed UI for `Credential Styling` on `Create Credential Type` / `Edit Credential Type`
- Bumped `meeco-sdk` to version `8.3.0-beta`

### Removed

- Removed the controllers for `credentialIssues`, `credentialTypes`, `credentialSchemas`, `presentationDefinitions`, `presentationRequests`, `shares`, `connections` and `Applications` and use the API directly.
- Removed the controllers for `User`, `TenantAdmin` and `OrganizationAdmin` and use teh API directly.
- Removed the controllers for `EndUser`, `Tenant` and `Organization` and use the API directly.
- Removed `SD Compatible` field on `View Credential` screen
- Removed `Wallet DID` column from `View Credentials` screen table
- Removed `Received` column from `Verification requests` screen

### Bug Fixes

- Fixed the alert error message when creating an organization
- Fixed pagination loading state, on error the pagination table would freeze
- Fixed `Create Organization` loading state and loading race conditions
- Fixed infinite loading looping when loading the `Verification Request` page
- Fixed stale `Verification Responses` from showing up when loading another `Verification Request`
- Fixed navigation on `Passphrase` submission, now navigates to `Organisation /dashboard`
- Fixed error on empty organisation list, preventing new `Organisations` from loading
- Fixed blank error message on `Create Organisation` page. Now returns default error message instead of `Error`


## Gateway

- KrakenD upgraded from version 2.8.3 to version 2.9.4. This is an important security upgrade which fixes the following vulnerabilities:
  - CVE-2025-30204: JOSE Library Memory Exhaustion (affecting JWT validation)
  - CVE-2025-22870: HTTP Proxy Bypass via IPv6 Zone Identifiers
  - CVE-2025-22871: Request Smuggling in Go’s net/http
  - CVE-2025-29923: Redis Out-of-Order Responses Risk
- Version of SVX Utilities added to GET /version.
- System status report of SVX Utilities added to `GET /system/status`.


## VC

### Added

- Support for background-image in CredentialTypeStyle
  - `POST /credential_types` accepted in payload
  - `PUT /credential_types` accepted in payload
  - `GET /credential_types/:id` added to response

### Fixed

- Added null safety to `presentationSubmission` and `presentationSubmission.descriptor_map` in `PEXPresentationSubmissionService` to prevent runtime errors with incomplete presentation submissions

## SVX Utilities

- Erlang upgraded to version 27
- Elixir upgraded to 1.18.3
- dependency upgrades
- improvements for monitoring SVX AMQP state
- a range of configuration improvements designed to support uniform configuration management across the platform.


## ATOM

- Erlang upgraded to version 27
- Elixir upgraded to 1.18.3
- dependency upgrades
- a range of configuration improvements designed to support uniform configuration management across the platform.

## Vault

- Rails 8
- Ruby 3.4.3. (security release)
- a range of configuration improvements designed to support uniform configuration management across the platform.
- AMQP report fixed


## Keystore

- Rails 8
- Ruby 3.4.3. (security release)
- a range of configuration improvements designed to support uniform configuration management across the platform.
