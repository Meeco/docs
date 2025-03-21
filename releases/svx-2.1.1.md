# SVX 2.1.1 Release Notes

**Software Release Date:** 18 March 2025

**Summary:**
This update includes a series of minor but important bug fixes across multiple components, improving navigation, error handling, and configuration management for a smoother user experience.

## Portal

### Bug Fixes

- Fixed navigation on `Passphrase` submission, now navigates to the Organisations dashboard
- Fixed error when creating the first Organisation within a Tenant, that prevented new Organisations from loading
- Fixed the blank error message when creating a new Organisation on the Create Organisation page. Now a default error will be displayed instead of a blank error

## SVX Utilities

### Bug Fixes

- Database credentials are read first from environment variables, and only then, from the configuration file if the environment variables are not set.
