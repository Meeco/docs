# SVX 1.3.7 Release Notes

**Software Release Date**: 01 August, 2024

**Summary**:

This release includes a security fix.

## Security

- Responses to requests handled by SVX IDP contain header `Cache-Control: no-store` to avoid caching potentially sensitive information.
