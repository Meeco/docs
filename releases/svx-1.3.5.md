# SVX 1.3.5 Release Notes

**Software Release Date**: XXX, 2024

**Summary**:  
This release introduces several enhancements and bug fixes across multiple SVX services, focusing on improved search capabilities, OpenAPI specification management, JWT-based token support, and enhanced security.

## Enhancements

### SVX Services

- **Search Features**:
  - Added search functionality for `GET /credential_types` and `GET /schemas` endpoints.
- **OpenAPI Specification**:
  - Added OpenAPI specification file to source control, requiring manual updates with `npm run save:openapi:spec`.
- **Schema Management**:
  - Added support for updating `organisation_ids` in archived schemas via `PUT /schemas/:id` endpoint.
- **Sorting and Attributes**:
  - Added sorting by `updated_at` for `GET /presentations/requests` endpoint.
  - Added `archived` and `version` attributes for `GET /credential_types` and `GET /credential_types/:id`.
  - Added `archived` attribute for `GET /presentation_definitions` and `GET /presentation_definitions/:id`.
- **JWT Token Support**:
  - Added support for the `x5c` header in JWT-based tokens for verification with the certificate’s public key.

### OIDC

- **OpenAPI Specification**:
  - Added OpenAPI specification file to source control, requiring manual updates with `npm run save:openapi:spec`.
- **RabbitMQ and Redis**:
  - Added custom health indicator for RabbitMQ connection status.
  - Added support for connecting to Redis store over TLS with new environment variables.
- **User Experience**:
  - Improved RP initiated logout screen functionality.
  - Updated PortalClientSeeder with new `logoutRedirects` values.

### Identity Network

- **OpenAPI Specification**:
  - Added OpenAPI specification file to source control, requiring manual updates with `npm run save:openapi:spec`.

### Vault

- **Database Management**:
  - Added `ON DELETE CASCADE` to foreign key constraint `orgs_user_id_fkey`.
- **Routes Update**:
  - Changed routes for OpenAPI and Swagger UI to `/openapi` and `/openapi/ui`.
- **Security Updates**:
  - Updated to Ruby 3.3.2 and Rails 7.1.3.4.

### Keystore

- **Versioning and Deployment**:
  - Updated versioning for changes in swagger file and important configuration updates.
  - Ensured service compatibility with read-only filesystem in K8S or docker-compose.

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

### SVX Services

- **OpenAPI Spec Fixes**:
  - Updated OpenAPI spec with missing search parameters for `GET /credential_types` and `GET /schemas` endpoints.
- **Schema Creation**:
  - Removed incorrect logic for `limit_disclosure` attribute and unnecessary `id` property requirement in `POST /schemas` endpoint.

### OIDC

- **Error Handling**:
  - Resolved issue in client credentials flow to return appropriate 401 error instead of 500 error when client is not UUID.

### Portal

- **Error Fixes**:
  - Fixed error when presentation definition is empty.
  - Updated `config.json` file to grab `otel` config instead of from `process.env`.
  - Removed unnecessary API calls in Credential Template page.

## Security

- **Upgrades**:
  - Upgraded NodeJS to LTS 20.13.1.
  - Upgraded NestJS dependencies, `@meeco/sdk`, `@meeco/sd-jwt-vc`, and other project dependencies to latest stable versions.

## Deprecations and EOL

- **OpenAPI File Generation**:
  - Removed generating and saving OpenAPI specification file at application startup due to read-only filesystem in the container.
