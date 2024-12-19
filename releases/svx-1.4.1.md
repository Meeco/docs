# SVX 1.4.1 Release Notes

**Software Release Date**: 21 August, 2024

**Summary**:

This release includes a security update to response headers in our Gateway. The addition of HTTP Strict-Transport-Security (HSTS) protects users from attacks and enhances trust when using websites.

## Gateway

- HSTS response header added to majority of responses. The HSTS informs browsers that the site should only be accessed using HTTPS, and that any future attempts to access it using HTTP should automatically be upgraded to HTTPS*. HSTS ensures that websites are only accessed over secure HTTPS connections, protecting against downgrade attacks, SSL stripping, and man-in-the-middle attacks. This enhances user security and trust by enforcing encryption and simplifying secure communication

\* **Source**: [Strict-Transport-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security) published by Mozilla Corporation.
