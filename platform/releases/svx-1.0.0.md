# SVX 1.0.0 Release Notes

**Software Release Date**: April 1, 2023

**Summary**: We are pleased to announce the release of the Secure Value Exchange (SVX) platform. SVX is designed to provide a seamless and secure way to store and exchange data between organisations and end-users, with comprehensive support for verifiable credentials. It includes a single API, a web portal, and a mobile identity wallet app.

The following is a summary of the key features and enhancements:

* **One API**: A unified API streamlines over 180 endpoints and 40 resources, organised into 14 groups, to provide a simple, secure, and efficient way to manage data, credentials, and create DIDs.
* **Portal**: The enterprise portal provides a centralised platform for managing tenants, organisations, and the complete life cycle of credentials. The intuitive and user-friendly interface enables organisations to streamline their processes and manage their credentials efficiently, while APIs provide advanced functionality and flexibility for those who need it.
* **Verifiable Credential Life-Cycle Management**: Easy entry for organisations to manage their credentials efficiently and securely, plus full flexibility to integrate directly with the API.
* **Identity Wallet**: Enhanced functionality and security for managing verifiable credentials, ensuring that end-users can manage their credentials efficiently and securely. The updated wallet now supports the latest set of standards in the community, enabling us to remain interoperable with other providers.
* **Updated Documentation**: More detailed and comprehensive information on features and capabilities, reducing the learning curve and streamlining the user experience.
* **Updated Typescript SDK**: The Meeco JS-SDK, now provides developers with enhanced functionality and flexibility for integrating the platform into their applications.
* **Enterprise Vaults**: Allows organisations to securely store sensitive data.

## New Features

### All Services In One Platform

We have released the Secure Value Exchange platform, which features a single API, portal web application, and mobile wallet application. This platform is designed to address the challenges of securely storing and exchanging data between organisations and end-users. It provides comprehensive support for verifiable credentials, allowing organisations and end-users to experience the complete end-to-end credential workflow via a user friendly portal and wallet.

### One API

We have recently streamlined our services and made them easily accessible through a single, user-friendly API. With over 180 endpoints and 40 resources, organised into 14 groups, you can now easily interact with the software and utilise all services with ease. This simplification aims to enhance user experience, increase efficiency, and reduce complexity for our partners. Whether you need to manage data, create DIDs, or manage credentials, the unified API provides a simple, secure, and efficient way to do so.

Endpoints are grouped as follows:

* **Classifications**: Organise data into classes.
* **Connections**: Create persistent, end-to-end encrypted connections between users and organisations.
* **Decentralised Identifiers (DIDs)**: Create, read, update and delete DIDs.
* **Delegations**: Grant another user the ability to access data.
* **Events**: Retrieve a log of events associated with actions in the system.
* **Items**: Store individual pieces of data, whether structured or unstructured, for secure and efficient storage and retrieval.
* **Keys**: Facilitate the storage and retrieval of encrypted keys.
* **Organisations**: Allow an enterprise, department, or group access with the purpose of exchanging information with end-users.
* **Security Rights**: Manage the permissions associated with tenants, organisations and their administrators.
* **Shares**: Securely exchange any type of data, with anyone.
* **Tenants**: Start your own personal data ecosystem.
* **Users**: Invite users to act as administrators or grant access to end-users.
* **Verifiable Credentials**: Verifiable credential life-cycle management.
* **Verifiable Presentations**: Present and verify verifiable credentials.

### Portal

The enterprise portal provides a simple interface for managing tenants, organisations, and the complete life cycle of credentials. You can easily create, manage, and monitor organisations, and ensure efficient management of your credentials throughout their life cycle. Whether you need to issue, verify, or revoke credentials, the portal provides a comprehensive solution to meet all your needs. The portal is designed to be user-friendly and intuitive, making your journey with personal data ecosystems as smooth as possible.

We understand that security is of utmost importance when it comes to managing sensitive information, and as such, we take great care to protect all cryptographic keys associated with the data stored on the platform. To ensure maximum security and privacy, all cryptographic keys are managed within the user's browser, and as the platform operator, we do not have access to these keys. This ensures that only the user has control over their data and can access it with the relevant keys, providing end-to-end security for all information stored on the platform. Our approach to key management is designed to provide the highest level of security while ensuring ease of use and seamless integration with the platform.

#### Tenant, Organisations and End-Users

The onboarding process enables efficient registration and onboarding of new organisations and end-users. Each organisation is assigned a unique DID to manage their keys securely. However, the keys used to control the DID are managed exclusively by the organisation's administrators, ensuring that sensitive data is only accessible to authorised personnel.

We take data privacy seriously, and we understand the importance of keeping sensitive information secure. As such, we only capture the DIDs of end-users, ensuring that their personal information is protected at all times. This approach allows us to provide a secure and efficient service while ensuring that no end-user data is leaked unnecessarily. We are committed to protecting the privacy of users and providing a secure platform that meets the highest standards of data protection.

#### Verifiable Credential Life-Cycle Management

The platform is designed to enable a quick start for organisations looking to manage their credentials efficiently and securely, while still offering the flexibility to integrate directly with the APIs. Whether you're just getting started with credential management or are using these features in production, the portal makes it easy to manage your credentials with ease. An intuitive and user-friendly interface enables organisations to streamline their processes and manage their credentials efficiently, while the APIs provide advanced functionality and flexibility for those who need it.

### Identity Wallet

We have updated the Meeco Wallet to work seamlessly with the new platform and its updated Verifiable Credential Life-Cycle Management system. The updated wallet provides enhanced functionality and security for managing verifiable credentials, ensuring that end-users can manage their credentials efficiently and securely.

#### Connect to Tenants and Organisations

The Meeco Wallet now enables users to connect to multiple tenants, giving them access to the services provided by each tenant (for example secure credential storage) and allowing them to interact with the organisations within each tenant.

#### Aligned to Open Standards

The wallet has been updated to support the latest set of standards in the community, ensuring that we remain interoperable with other providers. For an up-to-date list of standards we align to, please go to the documentation pages.

### DID Resolver & Registrar

The platform provides a DID Resolver & Registrar service for efficient and secure DID resolution and registration. DID resolution retrieves a DID document from a DID, while DID registration creates, updates, or deactivates a DID. We already support a range of DID methods that fits everyone's needs.

### Verifiable Credential Service

The Verifiable Credential Service is a complete rewrite of our previous credential service, providing enhanced functionality and security for managing verifiable credentials. The service includes generation, presentation, and verification of verifiable credentials, ensuring secure and efficient credential management.

The platform supports the OIDC Verifiable Credential suite of emerging standards, which are gaining widespread adoption across the industry. By adhering to these standards, we ensure interoperability with a broad range of parties and pave the way for an open data ecosystem.

### Fine-Grained Permission Management

Fine-grained permission management features apply to tenants, organisations, and their administrators, providing granular control of the features they are able to use. This ensures a flexible configuration for every situation.

## Enhancements

### Added Enterprise Vaults

In this release, we have introduced the ability for organisations to have a vault. This feature provides a secure and convenient location for organisations to store sensitive data, such as verifiable credentials and other confidential information. The vault can only be accessed by authorised personnel within the organisation, ensuring that sensitive data remains protected at all times.

### Updated Documentation

We have updated the documentation to provide more detailed and comprehensive information on our platform's features and capabilities. This update ensures that users have all the information they need to use the platform effectively and efficiently, reducing the learning curve and streamlining the user experience.

### Updated Typescript SDK

We have updated the Meeco JS-SDK, providing developers with enhanced functionality and flexibility for integrating the platform into their applications. The updated SDK provides a more intuitive and streamlined approach to integration, reducing complexity.

## Deprecations and EOL

* It is no longer possible for users to use SRP (Secure Remote Password) authentication.
* The Verifiable Credentials service no longer allows an option for managing keys, ensuring users have full control over their cryptographic keys.
* Authorisation is managed internally. This means that the platform handles the authentication and authorisation of users and applications, ensuring a more reliable user experience.