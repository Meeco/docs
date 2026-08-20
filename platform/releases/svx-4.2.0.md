# SVX 4.2.0 Release Notes

**Software Release Date:** 20 August 2026

**Summary:**

This is a feature release for the SVX Platform, with a privacy fix for batch credential issuance and a new setting to control credential timestamp precision.

## New Features

- You can now control how precise the timestamps on issued credentials are, via the new `issuer.issuance_policy.time_claims_granularity` setting. By default they're rounded to the nearest hour (3600 seconds), but you can change that window or set it to `1` for second-precision timestamps. This is configurable from the issuance policy section of the Admin UI.

## Improvements

- The configuration reference now documents a few OAuth/OIDC client settings that were already supported but not written down anywhere.

## Bug Fixes

- Fixed a privacy issue in batch credential issuance. When several credentials were issued together in one request, each one got its own precise issuance timestamp. Because those timestamps were so close together and so specific, a verifier could use them to work out which credentials came from the same batch and link presentations back to the same person. Credentials issued together now all get the same, rounded timestamp, so they can no longer be singled out this way.
- Verifiers can now include extra, type-specific fields when attaching transaction data to a presentation request, instead of having the request rejected.
- Metrics requests no longer get written to the logs, cutting down on noise.
