Tenant and Organisation Administrators can provide an application with authenticated access to SVX functionality. This allows the application to perform different actions based on the permissions of the Tenant or Organisation Administrator creating the application.

Applications created by Tenant Administrators will be able to:
* Invite and remove end-users
* Manage credential schemas

Applications created by Organisation Administrators will be able to:
* Create and archive credential templates
* Issue and revoke credentials
* Create and archive verification templates
* Create verification requests and view verification responses
* Create and cancel connections with end-users

## Access the SVX Sandbox API and create an application

To access the SVX Sandbox API and create an application you will need to follow the steps below:

#### 1. Access the SVX Sandbox API documentation

Navigate to [SVX Sandbox API documentation](https://api-reference-sandbox.svx.exchange/). At the top of the landing page, you will see the OpenAPI3 specification. Download the specification and import it into [Postman](https://learning.postman.com/docs/integrations/available-integrations/working-with-openAPI/).

> **Note**
> To download the OpenAPI3 specification into Postman, follow these simple steps:
> 1. Download the specification from the [SVX API documentation](https://api-reference-sandbox.svx.exchange/)
> 2. Open Postman
> 3. Import the downloaded .json file

#### 2. Create an application

Log in to the [SVX Portal](https://portal-sandbox.securevalueexchange.com/login) and create an application, see the Applications tutorial located in the [Portal tutorials](guides/portal-tutorials/README.md) for more information. 

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

> **Note**
> The Application Token will expire every 10 minutes. To refresh the token you will need to call the token endpoint again.

#### 4. Access the SVX Sandbox API

With the obtained authorisation token, you can now use the SVX Sandbox API. For example, if you want to access the me endpoint, use the following cURL command:
```bash
   curl --location 'https://api-sandbox.svx.exchange/me' \
   --header 'Meeco-Organisation-Id: YOUR_ORGANISATION_ID' \
   --header 'Accept: application/json' \
   --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_ORGANISATION_ID` with the relevant ID for your organisation, and `YOUR_ACCESS_TOKEN` with the token obtained in the previous step. You should now be able to successfully access the SVX Sandbox API using the provided authorisation token. 
