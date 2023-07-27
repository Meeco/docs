# Authorisation, Tenant and Organisation Manager

Meeco's Authorisation, Tenant and Organisation Manager (ATOM) enables the creation of a hierarchy of organisations within the Secure Value Exchange (SVX) platform. These organisations have associated administrator users, which are defined as Tenant Administrators and Organisation Administrators. Dependent on the role, an Administrator will have a set of security rights which denote what actions they can undertake. ATOM manages not only Administrator security rights but also the security rights of the Tenants and Organisations as individual entities. In doing this, ATOM allows Meeco components to authorise the actions of all parties within the ecosystem. This creates a robust, secure and accountable exchange of data between the different parties.

## Security Rights

Security rights control permissions granted to tenants, organisations, and their administrators. These rights determine what specific activities and operations each entity can perform, ensuring proper access control and data protection.

For tenants, security rights govern their overarching privileges, such as managing and provisioning organisations, end-users, and managing credential schemas. Organisations, as subsets of tenants, have security rights that pertain to data management within the tenant environment, including issuance and verification of verifiable credentials.

Administrators, who oversee the tenant and/or organisation, are granted security rights that enable them to perform administrative tasks, manage user access, and configure system settings. By effectively managing security rights, the platform ensures that each entity has the appropriate level of access and control over the system's functionalities while maintaining the necessary security and privacy measures.

## Applications for M2M Communication

In addition to user interactions, the platform facilitates machine-to-machine (M2M) communication, enabling applications to call the platform's APIs. Applications interact on behalf of an organisation. By leveraging the platform's capabilities for machine-to-machine communication, organizations can achieve greater efficiency, scalability, and interoperability in their application ecosystems.

## Event Logs

ATOM is built as an event system, providing numerous benefits, including comprehensive auditing capabilities. The event-driven architecture of ATOM enables the capture and logging of various events that occur within the system. By logging events, ATOM allows for detailed auditing and tracking of actions, providing a valuable tool for compliance, security, and accountability purposes.
