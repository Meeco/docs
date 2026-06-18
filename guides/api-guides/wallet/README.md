# SVX Wallet

The SVX Wallet is a service that provides digital wallets inside an SVX tenant environment. A single instance manages any number of wallets, each of which is a collection of cryptographic keys, [DIDs](../dids.md) and credentials. The service can be integrated with a mobile or web application, or with a third-party service, via its REST API and client SDKs (JavaScript, Kotlin and Swift).

Beyond holding credentials, the same service provides the Issuer and Verifier capabilities: it can create credential offers and issue credentials, and it can create presentation requests and verify the credentials presented in response. These capabilities are enabled per deployment, so an SVX Wallet can act as a holder wallet service, an issuing and verifying service for an Organisation, or all of these at once.

All cryptographic keys are managed by the service on behalf of its wallets (custodial key management) and are generated and protected by the configured Key Management Service (KMS). Keys are used to control identifiers (such as DIDs) and to perform key binding in credentials.

The SVX Wallet implements the leading standards in the space, with a focus on the OpenID4VC family of specifications (OpenID4VCI for issuance, OpenID4VP for presentation), JWT-based credential formats, and optionally DIDs.

Administrators can manage a deployment through the SVX Wallet Dashboard, a web application served by the service at `/admin`, or directly through the Wallet API.

## Integrating with the API

If you are building against an SVX Wallet deployment, the typical path is:

1. **Authenticate** — obtain an access token. See [Using the Wallet API](using-the-api.md).
2. **Create a wallet and a key** — set up the wallet and the holder key credentials bind to. See [Manage Wallets and Keys](manage-wallets.md).
3. **Add credentials** — receive credentials from an issuer or import them directly. See [Add Credentials to a Wallet](add-credentials.md).
4. **Present credentials** — respond to a verifier's request with a verifiable presentation. See [Present Credentials](present-credentials.md).

## Deploying the service

If you are standing up or operating an SVX Wallet deployment, see [Configuration](configuration.md) for the static configuration file and runtime settings.
