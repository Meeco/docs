# SVX 1.3.6 Release Notes

**Software Release Date**: XXX, 2024

**Summary**:
This release introduces several enhancements and bug fixes across multiple SVX services, focusing on improved search capabilities, OpenAPI specification management, JWT-based token support, and enhanced security.

## Enhancements


### IDP

- **RabbitMQ**:
  - IDP can now connect to RabbitMQ over TLS
  - Added custom health indicator for RabbitMQ connection status.
- **Redis**:
  - IDP can now connect to Redis over TLS
- **User Experience**:
  - Improved RP initiated logout screen functionality.
  - Updated PortalClientSeeder with new `logoutRedirects` values.

### VC

- **Changes**:
  - `presentation_definition.format` moved under the `presentation_definition.input_descriptors[index].format` key.
    - Impacted endpoints:
      - `POST /oidc/presentations/requests`
- **Added**:
  - Search feature added to `GET /credential_types`.
  - Search feature added to `GET /schemas`.
  - `organisation_ids` update support added to the archived schema via `PUT /schemas/:id` endpoint.
  - Sorting attribute `updated_at` added to `GET /presentations/requests`
  - Attributes `archived` and `version` added to endpoint `GET /credential_types` and `GET /credential_types/:id`
  - attributes `archived` added to `GET /presentation_definitions` and `GET /presentation_definitions/:id`
  - Support for the `x5c` header added for JWT-based tokens.
  - Support for verification of the following attributes in the presentation request, as defined by the OID4VP draft20 specification:
    - `client_metadata`
    - `response_uri`
    - Add support for optional `input_descriptors` (`input_descriptors[].optional`).
    - Add support for `response_mode` = `direct_post`.
    - Make `claims` attribute optional.
    - Impacted endpoints:
      - `POST /oidc/presentations/request/verify`
      - `POST /oidc/presentations/response/verify`
  - Support added for optional `presentation_submission` param for when `response_type` = `vp_token`
    - Impacted endpoints:
      - `POST /oidc/presentations/response/verify`
  - `POST /credentials/generate` endpoint accepts strings containing `.:-_/+` special characters for `type` payload param.

### Gateway

- **Upgrades of Base Software**:
  - KrakenD upgraded to version 2.6.3
- **Configuration Improvements**:
  - All configuration is now done via configmaps, no more configuration in images
  - New configuration variable: `default_global_timeout`
  - Version of the notifications microservice is now available in `GET /version`

### Identity Network

- **OpenAPI Specification**:
  - Added OpenAPI specification file to source control, requiring manual updates with `npm run save:openapi:spec`.


### Notifications

- **RabbitMQ**:
  - The notifications service can now connect to RabbitMQ over TLS
  - Custom health indicator added for RabbitMQ connections. The status endpoint now includes RabbitMQ connection status.

### ATOM

- **File Management**:
  - AWS S3 support in addition to Azure Blob Storage
  - One ATOM instance can operate multiple file storage backends

### Vault

- **Documentation**:
  - Internal routes for OpenAPI and Swagger UI changed to /openapi and /openapi/ui

### Keystore

- **Documentation**:
  - Internal routes for OpenAPI and Swagger UI changed to /openapi and /openapi/ui

### Portal

- **Back-end Enhancements**:
  - Updated header using `upload_headers` from BlobResponse in `POST /blobs` endpoint.
  - Implemented sorting attributes `createdAt` and `updatedAt` for `GET /presentations/requests` endpoint.
  - Added back-end search for Verification Requests page.
  - Implemented `issuer` field in `POST /presentation_definitions`.
  - Utilized `meeco/sdk` for handling credential definition requests.
- **SVX Compatibility**:
  - Displayed `sd-jwt-vc` format in credential detail page.

## Bug Fixes

### VC

- **OpenAPI Spec Fixes**:
  - Updated OpenAPI spec with missing search parameters for `GET /credential_types` and `GET /schemas` endpoints.
- **Schema Creation**:
  - Removed incorrect logic for `limit_disclosure` attribute and unnecessary `id` property requirement in `POST /schemas` endpoint.

### IDP

- **Client Credentials Flow**:
  - when the client is not UUID, the service does not return 500 error anymore.
    Instead, it returns 401 error with the message "client authentication failed".

### OIDC

- **Error Handling**:
  - Resolved issue in client credentials flow to return appropriate 401 error instead of 500 error when client is not UUID.

### Keystore

- **Errors Fixed**:
  - It is now not possible to create 2 key encryption keys for one user
  - it is now not possible to create 2 derivation artefacts for one user

### ATOM

- **Errors Fixed**:
  - No exception is thrown when the fallback JWK in the app configuration is not equal to the JWK from the webpage

### Vault

- **Errors Fixed**:
  - Added `ON DELETE CASCADE` to foreign key constraint `orgs_user_id_fkey`
  - Bug processing incoming classification parameters fixed

### Portal

- **Errors Fixed**:
  - Fix the 500 error during get credential response
  - Fix the error message with exist application
  - Fix can not open tenant/organisation after creation
  - Fix the landing page not correctly when user has one tenant and one organisation
  - Disable the revoke button instead of hidding it
  - Fix the logic of security container in credential detail page
  - Fix the connections display in new organisation issue
  - Fix the error when presentation definition is empty
  - Fix the create/archive/restore function in `credential request ` page

## Security

- **Identity Network Upgrades**:
  - NodeJS upgraded to the latest LTS `20.13.1`
  - `@nestjs/*` packages upgraded
  - Other packages upgraded to the latest stable version

- **IDP Upgrades**:
  - NodeJS upgraded to the latest LTS `20.13.1`
  - `@meeco/sdk` package upgraded to version `5.1.0`
  - `@nestjs/*` packages upgraded
  - `oidc-provider` package upgraded to version `8.4.6`
  - Other packages upgraded to the latest stable version

- **VC Upgrades**:
  - NestJS dependencies upgraded
  - `@meeco/sdk` upgraded to `5.0.0-beta`
  - `@meeco/sd-jwt-vc`  upgraded to version `1.2.2`
  - Other project dependencies upgraded

- **ATOM Container Security**:
  - Service runs under a non-privileged user
  - Service can run on a readonly filesystem

- **ATOM Upgrades**:
  - Base docker image upgraded to Debian 12
  - OTP version 26.2.5
  - Elixir version 1.16.3
  - Project dependencies upgraded

- **Keystore Container Security**:
  - Service runs under a non-privileged user
  - Service can run on a readonly filesystem

- **Vault Container Security**:
  - Service runs under a non-privileged user
  - Service can run on a readonly filesystem

- **Vault Upgrades**:
  - Base docker image upgraded to Debian 12
  - Ruby upgraded to version 3.3.3
  - Rails upgraded to version 7.1.3.4
  - Project dependencies upgraded

- **Keystore Upgrades**:
  - Base docker image upgraded to Debian 12
  - Ruby upgraded to version 3.3.3
  - Rails upgraded to version 7.1.3.4
  - Project dependencies upgraded


## Deprecations and EOL

- **OpenAPI File Generation**:
  - Removed generating and saving OpenAPI specification file at application startup due to read-only filesystem in the container.

- **VC**:
  - Logic for presentation request `limit_disclosure` attribute removed as incorrect.
  - Remove the requirement for an `id` property to exist when creating a new schema via `POST /schemas` endpoint.
