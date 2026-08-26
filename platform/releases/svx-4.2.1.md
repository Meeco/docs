# SVX 4.2.1 Release Notes

**Software Release Date:** 24 August 2026

**Summary:**

This is a security release for the SVX Platform.

## Security Fixes

- SD-JWT presentation verification now rejects any Disclosure that isn't referenced by a digest in the payload, instead of silently dropping it and letting the rest of the presentation pass.
- Disclosures and other JWT parts containing invalid UTF-8 are now rejected outright, instead of being silently decoded in a lossy way.
- Presentations containing the same disclosure more than once are now rejected with a clearer, dedicated error message.
