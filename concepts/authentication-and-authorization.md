# Authentication and Authorisation

Authentication and authorisation are two terms that are often used in conjunction when talking about identity, access management and security. They are both key aspects of digital trust to partake in a digital ecosystem.

## Authentication
Authentication is the process to authenticate and validate the identity of a user or entity. Authentication techniques and their level of complexity vary depending on the implementation itself and the security levels required. Commonly used techniques include:

* Username and password: This is the most common authentication method, where users provide a username and password to access a system.
* Two-factor authentication (2FA): This method adds a layer of security by requiring a second factor in addition to a password, such as facial recognition, PIN or a code sent to a mobile device.
* Multi-factor authentication (MFA): This requires more than two factors for authentication, typically something a user knows (such as a password), something a user has (such as a mobile phone), and something a user is (such as a biometric scan of their fingerprints or face).
* Single sign-on: This method allows the user to authenticate once and access multiple systems without the need to re-authenticate.

Authentication is vital for protecting user assets in a variety of domains and disciplines, including, but not limited to online banking, medical interactions, insurance, education and social networks. As technology advances, so does the risk of identity theft and fraud in these domains. There are moves towards strong authentication methods which bind a verified person to a digital account. Currently in Europe, these authentication methods enable individuals to undertake tasks online by utlising their devices which are bound to their digital credentials. These digital credentials prove that the device being used does belong to the individual and not someone claiming to be them.

## Authorisation
Authorisation is the process of granting or denying access to a resource.
When an entity requires access to a service, information or resources, they must be authorised before they can proceed. For a user to gain access to their digital accounts or data linked to their digital identity requires the custodian of these accounts / data to first authenticate the user, then determine whether, based on the provided credentials, they meet the requirements for authorisation. To determine whether the provided credentials meet requirements, the custodian and the user must establish a secure, trusted connection and exchange information that can be verified.

## Standard Protocols

OAuth (Open Authorization) and OIDC (OpenID Connect) are both protocols that are used for authentication and authorisation in the context of web and mobile applications.

OAuth is primarily an authorisation protocol that allows users to grant third-party applications access to their resources on a web service, without them having to share their credentials with a third party. The element here is the access token that is returned to the user and allows a third-party application (e.g. website) to access resources on their behalf.

OIDC, on the other hand, is a standard protocol that is built on top of OAuth and provides additional functionality for authentication. OIDC allows users to authenticate with a service using a variety of OAuth flows, and then provides them with an identity token (commonly referred to as id token), which contains information about the user, such as their name, email address or other PII attributes. This allows the third-party application to identify the user and personalise the experience based on that information.