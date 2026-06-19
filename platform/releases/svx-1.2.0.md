# SVX 1.2.0 Release Notes

**Software Release Date**: July 10, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Archiving**: Administrators now have the ability to archive and reinstate Organisations, Tenants and child entities
* **M2M Application for Tenants**: Enables machine-to-machine communication within a tenant's context
* **Differentiate End-user and Users in API**: Improved clarity in dealing with users and end-users
* **End-User Vault per Tenant**: Improved segmentation and security in handling data for end-users

## New Features

## Archiving and Reinstating

In this update, we have introduced archiving and reinstating of objects accross the board. By archiving, the object becomes unavailable for new actions while preserving its validity for past actions. Reinstating reverses the action and restores the object in its original state. This functionality assists organisations in managing their compliance duties.

We've taken the perspective to not apply cascading when archiving an object. The main reason is that it would be difficult to reverse the actions and restore the object in its previous state.

Note that by archiving an object, access to the object can become impossible (e.g. tenant, organisation).

Archiving and reinstating is available for the following objects

* Global Administrators
* Tenants
    * Tenant Administrators
    * Credential Schemas
    * End-users
* Organisations
    * Organisation Administrators
    * Credential Templates
    * Verification Templates

On the API side, this is implemented consistently by

* `POST /path/to/object/archive`: Archives the object
* `POST /path/to/object/restore`: Reverses the archiving operation, also called "reinstate"
* `GET /path/to/object?status=all|active|archived`: Get objects filtered by different states to suit any use case

In the Portal, archived objects are always presented in a separate tab on the same screen.

### Tenants & Organisations

When a Tenant or Organisation is archived, it triggers a comprehensive lockdown of all associated resources. All active Administrators, End-Users, and Applications lose their access to the Tenant and its associated Organisations, as well as the objects managed by the Tenant or Organisation. This means that no new operations or changes can be made within the archived Tenant, and all ongoing activities cease. This protective measure ensures the integrity and security of the Tenant/Organisation's data while archived.

### Tenant & Organisation Administrators

We've introduced the ability to archive both Tenant and Organisation Administrators. This operation is specific to the individual tenancy or organisation and does not impact the administrator's access to other tenancies or organisations. If an Administrator is removed from all Tenancies and Organisations, they will lose all access to the Portal.

## Tenant Application

Tenant applications leverage OAuth2's client credentials grant, a protocol designed for machine-to-machine (M2M) authentication. In this setup, a client ID and secret are used to authenticate and obtain an access token. This token, representing the specific application rather than a user, is then used to authorize the application to perform specific operations such as creating credential schemas, managing end-users, and tenant administrators within the context of a particular tenant. This approach enables secure and controlled access for services.

## Enhancements

### Separate End-user for Users in API

For improved clarity and efficiency, we have differentiated between users (admins) and end-users (like Meeco Wallet users) within our system architecture. Originally, these two groups were managed together via the same resource (/users), which created confusion due to their different roles and intentions.

To rectify this, we have isolated end-user management into its own specific endpoints. Here are the endpoints for managing end-users:

* `GET /end_users`: To fetch all end-users.
* `GET /end_users/{id}`: To fetch a specific end-user based on their ID.
* `GET /end_user/whoami`: For an end-user to identify themselves within the system.
* `POST /end_users/invitations`: To send an invitation to a new end-user.
* `GET /end_users/invitations`: To fetch all end-user invitations.
* `GET /end_users/invitations/{id}`: To fetch a specific end-user invitation based on its ID.
* `DELETE /end_users/invitations/{id}`: To revoke a specific end-user invitation.
* `DELETE /end_users/invitations/{token}/accept`: To reject an end-user invitation.
* `POST /end_users/invitations/short_lived_access_token`: To create a temporary access token for an end-user.
* `POST /end_users/{id}/deletion_queue`: To place a specific end-user in the deletion queue.
* `POST /end_user/deletion_queue`: Allow an end-user to place themselves in the deletion queue.

This new management structure provides clarity and precision, making it simpler for administrators to handle end-user interactions effectively.

### End-User Vault per Tenant

In our updated system, end-users are now granted a separate vault for each tenant, or provider, they are associated with. This ensures that user data is segmented and securely maintained within the context of the specific provider, enhancing both security and organization.

Furthermore, our system allows for cross-tenant connections. This capability provides flexibility and broadens the scope of interactions, enabling end-users to connect with various organizations across different tenants.

It is important to note that when an end-user switches providers within the wallet, their connections to organizations are also managed on a per-tenant basis, as these connections form part of the individual vault. This allows for seamless management of connections, ensuring the integrity and accuracy of each user's associations when transitioning between different providers.

### Various

* It is now possible to change your password as an administrator
* Revised toast messages in Portal
* Improved onboarding screens in Wallet
* Add ability to correctly display issuer as object or string in a VC in Wallet

## Deprecations and EOL

* Endpoints in the '/users' namespace used exclusively for end-users are now marked as deprecated
