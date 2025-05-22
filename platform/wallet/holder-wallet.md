The Holder Wallet (HW) is a key component of Meeco’s SVX Platform, designed to empower end users (Holders) to receive, claim, and manage issued Verifiable Credentials (VCs), as well as present them for verification. Accessible via the HW API, it is built on international standards and specifications to ensure interoperability across VC ecosystems. This enables Holders to securely share their information with trusted parties within global ecosystems that follow the same framework. By leveraging the HW, organisations can provide their customers with greater control over their data and the ability to share it seamlessly in everyday scenarios.

### Creating a HW

To create a HW a user must first sign up to Meeco’s SVX Platform and register as a Tenant Administrator (TA). After receiving SVX login credentials, access to the SVX API and the Portal will be possible. The TA will need to create an Application, see the [Machine-2-Machine Communication](../../guides/api-guides/machine-2-machine-communication.md) guide for API access or the [Applications](../../guides/portal-tutorials/tenant-administrators/applications.md) guide if using the Portal. Once an Application has been created, a TA will need to provide Meeco’s SVX team with the following information:
- Client ID
- Client Secret

From here, the SVX team will deploy a HW and an instance of the HW-FE connected with the Tenant’s application. Additionally, Tenant Administrators can specify which IdP they would like to associate with their HW. The IdP will enable end user (Holder) authentication at the time a Holder creates a wallet account via the HW-FE application. The IdP will then be configured to communicate with the HW (via the HW Gateway) and enable TAs visibility on the number of HW-FE users and their wallet IDs.

> **Note**
> TAs can use the [HW-FE provided by Meeco](#holder-wallet-front-end-hw-fe) or opt for their own or a third-party application.

### Verifiable Credentials (VCs)

Built on the international specifications [OpenID for Verifiable Credential Issuance (OID4VCI)](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OpenID Connect for Verifiable Presentations (OID4VP)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) the HW receives and presents VCs based on the [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/). As more organsiations and ecosystem operators adopt these specifications and standards, VC issuance, verification and Holder engagement becomes easier to initiate across platforms.

### Use Case Application

As the HW is provided as an API, its application is virtually limitless. Multiple HW instances can be utilised within a use case to distinguish between different Holders and their respective workflows. Additionally, given the wide variety of Verifiable Credentials (VCs) that can be issued and verified, its use extends across diverse industries and sectors.

For more information on use cases where the HW plays a pivotal role in the receiving, management and presentation of VCs, see our [Use Case Videos](https://www.meeco.me/resources/use-case-videos) via the Meeco website.

### Diagram

The diagram below provides a high-level overview of how the HW can interact with different services.

<p align="center">
<img align="center" src="/.gitbook/assets/Platform_HW_01_Holder_Wallet_Service_Diagram.png" alt="Holder Wallet service diagram." width="80%">
</p>

> **Note**
> As the HW is delivered as an API, all updates and changes are instant. New versions of the API will be released intermittently with associated updates communicated with customers promptly.

## Holder Wallet Gateway

The Holder Wallet Gateway sits in front of the HW and is a separate application designed to restrict API calls and make the HW API more secure. It does this by providing an authentication layer to the HW. The software used to create this Gateway is [KrakenD](https://www.krakend.io/) and its purpose is to manage API traffic, implement rate limiting, instill authentication measures, and serve as a control point for managing API requests. This layer can also enforce security policies and provide additional logging and monitoring capabilities.

Endpoints exist in the HW that are required to be publicly available. This is to ensure [OID4VCI](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) compliance, and for key and metadata lookup. There are also endpoints that must remain private when controlling and creating credential offers. The Gateway handles the associated authentication to only allow authorised requests to call these private endpoints.

## Holder Wallet Front End (HW-FE)

As HWs are primarily designed for use by people, it is recommended that a user-friendly interface is developed to facilitate wallet interactions. This interface can be implemented as a browser extension, a desktop application, or, more commonly, a mobile wallet app. Within SVX, the Holder Cloud Wallet (API) SDK allows customers to build their own wallet UI, plugins, or other integrations whilst utilising all wallet functionality.

Meeco has also developed a front-end mobile application (reference implementation) for the HW titled the Meeco Wallet. This mobile app is available for SVX customers when undertaking POCs and Pilot programs. Additionally, the user interface (UI) is somewhat customisable, including the addition of logos, colour themes and feature images. To view the functionality of the Meeco Wallet, see the [Wallet tutorials](./guides/wallet-tutorials).
