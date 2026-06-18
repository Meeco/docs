# SVX Wallet

The SVX Wallet is a service that provides digital wallets inside an SVX tenant environment. A single instance manages any number of wallets, each of which is a collection of cryptographic keys, [DIDs](../dids.md) and credentials. The service can be integrated with a mobile or web application, or with a third-party service, via its REST API and client SDKs (JavaScript, Kotlin and Swift).

Beyond holding credentials, the same service provides the Issuer and Verifier capabilities: it can create credential offers and issue credentials, and it can create presentation requests and verify the credentials presented in response. These capabilities are enabled per deployment, so an SVX Wallet can act as a holder wallet service, an issuing and verifying service for an Organisation, or all of these at once.

All cryptographic keys are managed by the service on behalf of its wallets (custodial key management) and are generated and protected by the configured Key Management Service (KMS). Keys are used to control identifiers (such as DIDs) and to perform key binding in credentials.

The SVX Wallet implements the leading standards in the space, with a focus on the OpenID4VC family of specifications (OpenID4VCI for issuance, OpenID4VP for presentation), JWT-based credential formats, and optionally DIDs.

Administrators can manage a deployment through the SVX Wallet Dashboard, a web application served by the service at `/admin`, or directly through the Wallet API. To learn how a deployment is configured, see [Configuration](configuration.md).
