# SVX 1.4.3 Release Notes

**Software Release Date**: 17 September, 2024

**Summary**:

In this release, the Gateway now includes the HSTS header in the GET / endpoint, which was missing in version 1.4.1. For the Portal, the default nginx configuration has been updated to hide version information by setting server_tokens off. The Content-Security-Policy has been revised to allow specific sources, ensuring better security. Additionally, a fix was applied to the add_header Content-Security-Policy command to correct its arguments, and new security headers have been added to the nginx configuration.

## Gateway

- HSTS header also added to `GET /` (was not included in 1.4.1).

## Portal

- **Changed:**
  - `Content-Security-Policy` value changed to `default-src 'self' 'unsafe-inline' 'unsafe-eval' data: blob: *.meeco.me *.meeco.cloud *.svx.exchange *.securevalueexchange.com *.windows.net *.amazonaws.com *.google.com *.gstatic.com *.googleapis.com cdn.jsdelivr.net`.
- **Added:**
  - `nginx` default config added with `server_tokens off` so that nginx version information is not returned via `Server` header value.
  - Security headers to 'ngix' config.
- **Fixed:**
  - `add_header Content-Security-Policy` called with correct number of arguments.
