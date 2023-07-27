To onboard to SVX you will first need to sign up. Navigate to our [sign up form](https://www.meeco.me/signup) to gain access to our Sandbox. Once your access has been approved, you will be able to [log in to the SVX Portal](https://portal-sandbox.securevalueexchange.com/login). From here you can either:
* Use the Portal UI to undertake workflows, or
* Directly access the SVX Sandbox API.

## Using the Portal UI

Navigate to the [Portal login page](https://portal-sandbox.securevalueexchange.com/login) and log in using your SVX credentials. Refer to the [Portal tutorials](guides/portal-tutorials/README.md) where you can find step-by-step instructions to navigate the Portal and manage a Tenancy and / or an Organisation.

## Accessing the SVX Sandbox API

To directly access the SVX Sandbox API you will need to follow the steps below:

#### 1. Access the SVX Sandbox API documentation:

Navigate to the [SVX Sandbox API documentation](https://api-reference-sandbox.svx.exchange/). At the top of the landing page, you will see the OpenAPI3 specification. Download the specification and import it into [Postman](https://learning.postman.com/docs/integrations/available-integrations/working-with-openAPI/).

> **Note**
> To download the OpenAPI3 specification into Postman, follow these simple steps:
> 1. Download the specification from the [API documentation landing page](https://api-reference-sandbox.svx.exchange/)
> 2. Open Postman
> 3. Import the downloaded .json file

#### 2. Retrieve a Personal Access Token

Open Postman and create a new request by clicking on the _New_ button in the upper-left corner.

Navigate to the _Authorization_ tab, which is located below the URL field.

In the _Authorization_ tab, select _OAuth 2.0_ from the _Type_ dropdown menu.

Fill in the OAuth 2.0 Access Token Request Details:

```bash
Grant Type: Authorization Code with PKCE
Auth Url: https://login-sandbox.securevalueexchange.com/oauth2/auth
Access Token URL: https://login-sandbox.securevalueexchange.com/oauth2/token
Client ID: ed3d2366-0fb6-406e-ae72-afd7634e6c9f
Scope: openid profile email offline_access
```

After filling in the required details, click on the _Request Token_ button. This will initiate the OAuth 2.0 authentication process.

You will be redirected to the authorisation page of the service you are connecting to. Log in using your SVX login credentials.

After successful authorisation, you will be redirected back to Postman, and the personal access token will be automatically saved.

#### 3. Access the SVX Sandbox API

With the obtained personal access token, you can now use the SVX Sandbox API. For example, if you want to access the ``me`` endpoint, use the following cURL command:

```bash

   curl --location 'https://api-sandbox.svx.exchange/me' \
   --header 'Meeco-Organisation-Id: YOUR_ORGANISATION_ID' \
   --header 'Accept: application/json' \
   --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'

```

Replace `YOUR_ORGANISATION_ID` with the relevant ID for your organisation, and `YOUR_ACCESS_TOKEN` with the token obtained in the previous step. You should now be able to successfully access the SVX Sandbox API using the provided authorisation token. 

For additional support, review the [API Guides](guides/api-guides/README.md) or contact the [Service Desk](https://meecosystem.atlassian.net/servicedesk/customer/portal/4).
