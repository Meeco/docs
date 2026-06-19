# SVX 1.3.7 Release Notes

**Software Release Date**: 29 July, 2024

**Summary**:

This release includes a security fix and a minor enhancements.

## Enhancements

- It is possible to disable file uploads in SVX via platform configuration. This impacts, for example, logo uploads for tenants, organisations and credential templates in the Portal. The user gets an error when trying to upload files if this flag is enabled. The default for this value is to allow file uploads.

## Security

- Responses to POST requests handled by the SVX API contain header `Cache-Control: no-store` to avoid caching potentially sensitive information.