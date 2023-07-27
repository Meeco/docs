To onboard to SVX you will first need to sign up. Navigate to our [sign up form](https://www.meeco.me/signup) to gain access to our Sandbox. Once your access has been approved, you will be able to [log in to the SVX Portal](https://portal-sandbox.securevalueexchange.com/login). From here you can either:
* Use the Portal UI to undertake workflows, or
* Create an application to obtain an access token for access to the SVX Sandbox API.

## Using the Portal UI

To navigate the Portal, refer to the Portal tutorials where you can find step-by-step instructions to manage a Tenancy and / or an Organisation.

## Accessing the SVX Sandbox API

To directly access the SVX Sandbox API you will need to follow the steps below:

#### 1. Access the SVX Sandbox API documentation:

Navigate to the [SVX Sandbox API documentation](https://api-reference-sandbox.svx.exchange/). At the top of the landing page, you will see the OpenAPI3 specification. Download the specification and import it into [Postman](https://learning.postman.com/docs/integrations/available-integrations/working-with-openAPI/).

> **Note**
> To download the OpenAPI3 specification into Postman, follow these simple steps:
> 1. Download the specification from the [API documentation landing page](https://api-reference-sandbox.svx.exchange/)
> 2. Open Postman
> 3. Import the downloaded .json file

#### 2. Create an application:

Log in to the [SVX Portal](https://portal-sandbox.securevalueexchange.com/login) and create an application, see the Applications tutorial located in the Portal tutorials section. 

> **Note**
> Tenant and Organisation Administrators can both create Applications, however, different workflows are achieved based on the associated role and access rights.

> **Note**
> Once an application has been created, ensure you record the ``client_id`` and ``client-secret``. 

#### 3. Open the API configuration

View the Open API configuration at https://login-sandbox.securevalueexchange.com/oauth2/.well-known/openid-configuration and retrieve your authorisation token.

Use the token endpoint from the configuration (https://login-sandbox.securevalueexchange.com/oauth2/token) to obtain an authorisation token. You can do this using the cURL command as shown below:
```bash
   curl -X POST https://login-sandbox.securevalueexchange.com/oauth2/token \
   -d 'grant_type=client_credentials' \
   -d 'client_id=YOUR_CLIENT_ID' \
   -d 'client_secret=YOUR_CLIENT_SECRET'
```
Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with the actual values you obtained when creating the application.

#### 4. Access the SVX Sandbox API

With the obtained authorisation token, you can now use the SVX Sandbox API. For example, if you want to access the ``me`` endpoint, use the following cURL command:
```bash
   curl --location 'https://api-sandbox.svx.exchange/me' \
   --header 'Meeco-Organisation-Id: YOUR_ORGANISATION_ID' \
   --header 'Accept: application/json' \
   --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```
Replace `YOUR_ORGANISATION_ID` with the relevant ID for your organisation, and `YOUR_ACCESS_TOKEN` with the token obtained in the previous step. You should now be able to successfully access the SVX Sandbox API using the provided authorisation token. 

For additional support, review the API Guides or contact the [Service Desk](https://meecosystem.atlassian.net/servicedesk/customer/portal/4).
