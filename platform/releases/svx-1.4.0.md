# SVX 1.4.0 Release Notes

**Software Release Date**: 16 August, 2024

**Summary**:

This release introduces several new functionalities, enhancements, and bug fixes aimed at improving the overall user experience and functionality of the system.

- **New functionalities**: Organisation Administrators can now view SD-JWT VC credentials, have more control over presentation template creation, and can set a Redirect Base URI. Tenant Administrators can create credential schemas using URLs to external schemas.
- **Enhancements**: Improved credential format determination, updated dependencies, increased end-user search length restriction, and added flexible database connection configuration options.
- **Bug Fixes**: Resolved various issues in the Portal, including problems with tenant editing, logo uploads, tab switching, credential template editing, and revocation button visibility.
- **Security updates**: Addressed a configuration issue in Portal related to IDP logout callbacks, ensuring proper session termination. Added missing security headers to the nginx configuration of the Portal service, enhancing security in environments without an application gateway handling this responsibility.

## New Functionality

### Portal

- Organisation Administrators can view SD-JWT VC credentials under issued credentials and in verification responses
- Organisation Administrators have more control over presentation template creation, allowing them to specify constraints for each of the required credentials which maps to `input_descriptors` in the presentation definition.
- Organisation Administrators can set a Redirect Base URI, enabling presentation responses to be sent to a configured domain for the organisation
- Tenant Administrators can create credential schemas using URLs to external credential schemas and JSON schema files

## Enhancements

### Portal

- Credential format determined using `header.typ` on the actual credential instead of the `format` property.
- Updated dependencies in Portal codebase.

### IDP

- End-user search length restriction increased from 100 to 512 characters. This allows to search for longer DIDs often used on end-users.
- Introduction of `PAGINATION_DEFAULT_PAGE_SIZE`, `PAGINATION_DEFAULT_ORDER`, `PAGINATION_MAX_SEARCH_QUERY_LENGTH` configuration options to control pagination behavior across endpoints. Defaults are 10, "DESC", 512 respectively.
- Upgraded NodeJS to the latest LTS 20.16.0

### Verifiable Credentials

- Changed `presentation_definition.required_credentials[].constraints.fields[].filter` parameter validation in `POST /presentation_definitions`. `type` is required, but `const` and `pattern` are optional, but can't appear both.

### All

- All services using a database are able to configure database connection using environment variables in addition to configuration files. This allows for more flexible configuration of database connections.
- All services show RabbitMQ configuration values in `GET /system/status` endpoint for easier monitoring of message queues and asynchronous processing.

## Bug Fixes

- Resolved an issue where organisation or tenant applications could still login after the organisation or tenant was archived. After archiving, allow for up to 10 minutes access token expiration.

### Portal

- Resolved an issue where not found error was displayed after clicking "Edit Tenant"  from the tenant list page.
- Resolved an issue that prevented logos to be uploaded.
- Resolved an issue that caused switching tabs, both having portal open, being stuck in a loop.
- Resolved an issue where edit credential template screen wasn't showing the schemas associated with the organisation.
- Resolved an issue where revocation button was visible, despite the credential not being revocable.
- Resolved an issue where empty tenant or organisation application description was causing a 400 error, despite successive attempts succeeding.
- Resolved an issue where state of create button was not updated correctly.

## Security

- Fix a configuration issue that caused Portal not to receive callback after successful logout from IDP session potentially causing unsuccessful session termination
- Add missing security headers to nginx configuration of the Portal service for environment where this responsibility is not taken care of by an application gateway.
