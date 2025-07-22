# CATRINA Identity Integration Guide

This guide outlines the process for integrating with the CATRINA Identity Platform. This document is still a work in progress and may contain errors, please reach out to Meeco for any help that you may require.

- [Overview](#overview)
- [Pre-requisites](#pre-requisites)
  - [Organisation Creation](#organisation-creation)
  - [Authentication (Applications)](#authentication-applications)
  - [Verification Template](#verification-template)
  - [Final Setup](#final-setup)
- [Integration and Authentication](#integration-and-authentication)
  - [Authentication](#authentication)
  - [Creating a Session](#creating-a-session)
  - [Retrieving a Session](#retrieving-a-session)
- [Integration Code Example (Javascript)](#integration-code-example-javascript)
  - [1. Add a button to your website (client-side)](#1-add-a-button-to-your-website-client-side)
  - [2. Redirect to CATRINA Identity (client-side, server-side)](#2-redirect-to-catrina-identity-client-side-server-side)
  - [3. Handle Result](#3-handle-result)
- [Access Verification Results](#access-verification-results)
  - [Integration Complete](#integration-complete)
- [Appendix 1: Verification Template Constraints Example](#appendix-1-verification-template-constraints-example)
- [Appendix 2: Available Attributes](#appendix-2-available-attributes)

## Overview

The CATRINA Identity Platform is designed for easy integration, giving organizations full control over their identity verification workflows. Integration starts in the Portal, where administrators can set key parameters, like the user data needed for a specific use case (e.g., name, date of birth, address, or government-issued ID), and define why this data is being requested.

Once configured, developers can use our standards-based REST API to start a secure data request session, get a unique session URL, and monitor the transaction status in real-time. To keep the integration lightweight and compatible with all systems, the only client-side task is to redirect users to the CATRINA-hosted experience, usually with a button or link.

When a user is redirected, CATRINA manages the entire identity verification process, including consent, data sharing, error handling, and secure transaction completion. The hosted environment follows privacy-by-design and security-by-design principles and complies with regulations like GDPR, eIDAS, and ISO/IEC 27001, so integrators don't have to.

After verification, users are sent back to your website or app. Verified identity attributes can be retrieved through the API or viewed on the dashboard. This approach offers the flexibility of an API-first model with the simplicity of a no-SDK, hosted solution. CATRINA is language-agnostic and works with any frontend framework, allowing integration into any environment without extra libraries or technical limitations.

## Pre-requisites

This section covers all the components that need to be set up before you can begin your CATRINA Identity integration.

### Organisation Creation

An Organisation must be created to successfully deploy your CATRINA identity integration. Organisations act as Issuers and Verifiers within the CATRINA Platform. For this integration, you'll need an organisation that acts as a Verifier.

Organisations are created by Tenant Administrators. If you are not a Tenant Administrator, please contact your CATRINA Identity account manager or raise a request at the DNP Service Desk.

Once your Organisation is created, you'll get an email invitation to join the Portal as an Administrator. Follow the instructions in the email to complete your onboarding and Organisation setup.

### Authentication (Applications)

To enable your integration, you need to set up two applications in the Portal. One application will host the CATRINA Identity application, and the other will be used to authenticate and connect to it.

**CATRINA Identity Hosting Application**

1.  Navigate to the "Devtools" tab in the side menu and select "Applications".
2.  Create an application with a suitable name and description, for example:
    - **Name:** "CATRINA Identity Host"
    - **Description:** "Used for the CATRINA Identity Platform"

**CATRINA Identity Authentication Application**

1.  Navigate to the "Devtools" tab and select "Applications".
2.  Create an application with a suitable name and description, for example:
    - **Name:** "CATRINA Identity Authentication"
    - **Description:** "Used for the CATRINA Identity Platform"
3.  After your CATRINA Identity Account Manager completes the setup, you can use the `client_id` and `client_secret` to securely connect to your CATRINA Identity deployment.

### Verification Template

Next, set up a Verification Template for your use case. This template is used to request specific information from your users.

1.  In the Portal, go to the "Verification templates" tab in the navigation menu.

![Select Verification Template](INV-01-select-template.png)

2.  Click the "Create verification template" button.

![Create Verification Template](INV-02-select-template-create.png) 3. Enter a template name and purpose. For example: - **Name:** "Guest Identity Verification" - **Purpose:** "To verify guest identity information using the CATRINA Identity Platform"

![Verification Template Name and Purpose](INV-03-template-name.png)

4.  Complete the "Required credential" section:
    - **Credential name:** Guest Identity
    - **Credential format:** Select `dc+sd-jwt`.
    - **Constraints:** This section is automatically populated. For account-based solutions, only specific attributes can be requested.
    - **Signature algorithm value:** This will be automatically populated. Leave it as is.

![Verification Template Credential](INV-04-template-credential.png) 5. Click "Create".

![Verification Template Create](INV-05-template-create-final.png) 6. Once created, click the ellipsis icon (...) and select "View" to get the `presentation_template_id`. You will need this for the session payload.

> **Note:** Despite the option to add multiple credentials via “Add required credentials”, the CATRINA Identity platform currently only supports one required credential per Verification Template. Please leave this as a single credential.

> **Note:** For account-based integrations, only specific credential attributes can be used when defining the credential “constraints”. See Appendix 2 for details.

### Final Setup

After completing these steps, contact your CATRINA Identity Account Manager with the following information:

- Your CATRINA Identity Hosting Application `client_id`.
- Your CATRINA Identity Authentication Application `client_id`.
- Your Presentation Template ID.

## Integration and Authentication

### Authentication

To interact with your CATRINA Identity integration, you'll need the `client_id` and `client_secret` from the "CATRINA Identity Authentication Application". This information is in the application's details in the Portal.

![Application Screen](INV-06-application.png)

You will also need the API login URL from the asset registry. The authentication call will look like this:

```bash
curl -X POST \
  "https://CATRINA_API_LOGIN/oauth2/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET"
```

This returns your `ACCESS_TOKEN`:

```json
{ "access_token": "ACCESS_TOKEN", "expires_in": 600, "token_type": "Bearer" }
```

> **Note:** Your application is required to use this ACCESS_TOKEN as a Bearer token in the Authorization header for all API calls to your CATRINA Identity deployment.

### Creating a Session

To create a session, you need:

- `presentation_template_id`: The `presentation_template_id` created beforehand
- `success_url`: The page the user returns to after a successful flow.
- `return_url`: The page the user returns to on cancellation or if an error occurs.

Create a session with this API call:

```bash
curl -X POST "https://YOUR_CATRINA_IDENTITY_URL/identity/sessions" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-H 'Content-Type: application/json' \
-d '{ "presentation_template_id": "YOUR_PRESENTATION_TEMPLATE_ID", "success_url": "[https://your-website.com/success](https://your-website.com/success)", "return_url": "[https://your-website.com/return](https://your-website.com/return)" }'
```

The response will be:

```json
{
  "id": "XXX-XXX-XXX-XXX",
  "url": "https://REDIRECT_URL"
}
```

Save the session `id` for future reference. The `url` is used to redirect the user to the CATRINA Identity website.

### Retrieving a Session

Once a session is complete, you can get the verification results from the `verified_claims` object in the session response.

Use the `session_id` to retrieve the data:

```bash
curl -X GET 'https://YOUR_CATRINA_IDENTITY_URL/identity/sessions/SESSION_ID' \
-H "Authorization: Bearer $ACCESS_TOKEN"
```

The response will be:

```json
{
  "session_id": "session_id",
  "status": "verified",
  "verified_claims": {
    "given_name_unicode": "Alice",
    "family_name_unicode": "Wong",
    "birth_date": "1993-06-15",
    "document_number": "ABC123456",
    "issuing_country": "JP",
    "expiry_date": "2030-01-01",
    "portrait": "data:image/jpeg;base64,...",
    "resident_address_unicode": "1-2-3 Shibuya, Tokyo",
    "age_over_18": true,
    "phone_number": "+819012345678",
    "email": "alice@example.com"
  },
  "error": "Error if exists",
  "error_description": "Error description if exists"
}
```

**Best Practices:**

- **Minimise data use:** Only process and store the fields you need.
- **Secure sensitive data:** Use secure channels with encryption.
- **Audit access:** Ensure only authorised services and personnel can access the data.

## Integration Code Example (Javascript)

This section provides a Javascript integration guide.

### 1. Add a button to your website (client-side)

```html
<html>
  <head>
    <title>Verify your identity</title>
  </head>
  <body>
    <button id="verify-button">Verify</button>
  </body>
</html>
```

### 2. Redirect to CATRINA Identity (client-side, server-side)

When the button is clicked, redirect the user to a CATRINA Identity hosted page.

**Sequence Diagram**
This diagram shows how a traditional backend and frontend application integrates with CATRINA Identity.
"Your" is the application integrating, and "Our" is the CATRINA Identity solution.

![CATRINA Identity integration sequence diagram](https://storage.googleapis.com/askg-public-images/5a676767-1644-4696-abf2-89241940a45e.png)

**Set up an endpoint to create a Verify Session**
On the server-side, create a `create-verification-session` endpoint (HTTP POST) that authenticates, creates a session, and returns the session URL.

```javascript
const axios = require("axios");
const { ClientCredentials } = require("simple-oauth2");

const config = {
  client: {
    id: "your_client_id",
    secret: "your_client_secret",
  },
  auth: {
    tokenHost:
      "[https://login.stg.svxawsjp.meeco.cloud](https://login.stg.svxawsjp.meeco.cloud)",
    tokenPath: "/oauth2/token",
  },
};

const api = axios.create({
  baseURL:
    "[https://ocw-kaze-no-hana-inv.catrina-identity.stg-diw.net](https://ocw-kaze-no-hana-inv.catrina-identity.stg-diw.net)",
  headers: { "Content-Type": "application/json" },
});

async function createVerificationSession(presentationTemplateId) {
  const client = new ClientCredentials(config);
  const token = await client.getToken();
  const res = await api.post(
    "/identity/sessions",
    {
      presentation_template_id: presentationTemplateId,
      success_url:
        "[https://your-website.com/success](https://your-website.com/success)",
      return_url:
        "[https://your-website.com/return](https://your-website.com/return)",
    },
    {
      headers: { Authorization: `Bearer ${token.token.access_token}` },
    }
  );
  return res.data.url;
}

// Usage
const url = await createVerificationSession("template-abc");
```

**Add an event handler**

> **Note:** Always create sessions on the server-side to protect your authentication keys.

```html
<html>
  <head>
    <title>Verify your identity</title>
  </head>
  <body>
    <button id="verify-button">Verify</button>
    <script type="text/javascript">
      var verifyButton = document.getElementById("verify-button");
      verifyButton.addEventListener("click", function () {
        fetch("/create-verification-session", {
          method: "POST",
        })
          .then(function (response) {
            return response.json();
          })
          .then(function (session) {
            window.location.href = session.url;
          })
          .catch(function (error) {
            console.error("Error:", error);
          });
      });
    </script>
  </body>
</html>
```

### 3. Handle Result

When the user is redirected back to your site, the verification is complete. You can retrieve results from the API or view them in the Portal.

## Access Verification Results

After a session is completed, access the structured verification results via the session response in the `verified_claims` object.

**Set up an endpoint to access a session's results**
On the server-side, create a `get-verification-session` endpoint (HTTP GET) to fetch session data.

```javascript
async function getVerificationSession(sessionId) {
  const client = new ClientCredentials(config);
  const token = await client.getToken();
  const res = await api.get(`/identity/sessions/${sessionId}`, {
    headers: { Authorization: `Bearer ${token.token.access_token}` },
  });
  return res.data;
}

// Usage
const sessionDetails = await getVerificationSession(sessionId);
```

**Example Response:**

```json
{
  "session_id": "sess_1234567890",
  "status": "verified",
  "verified_claims": {
    "given_name_unicode": "Alice",
    "family_name_unicode": "Wong",
    "birth_date": "1993-06-15",
    "document_number": "ABC123456",
    "issuing_country": "JP",
    "expiry_date": "2030-01-01",
    "portrait": "data:image/jpeg;base64,...",
    "resident_address_unicode": "1-2-3 Shibuya, Tokyo",
    "age_over_18": true,
    "phone_number": "+819012345678",
    "email": "alice@example.com"
  }
}
```

### Integration Complete

By following these steps, you should have successfully integrated your system with the CATRINA Identity platform.

## Appendix 1: Verification Template Constraints Example

Replace `"const": "PhotoID_wContact_INV"` with the Credential Type you are verifying against. Confirm this with your Meeco Account Manager.

```json
{
  "limit_disclosure": "preferred",
  "fields": [
    {
      "path": ["$.vct"],
      "filter": {
        "type": "string",
        "const": "PhotoID_wContact_INV"
      }
    },
    { "path": ["$.given_name_unicode"] },
    { "path": ["$.family_name_unicode"] },
    { "path": ["$.birth_date"] },
    { "path": ["$.phone_number"], "optional": true },
    { "path": ["$.email"], "optional": true },
    { "path": ["$.resident_address_unicode"], "optional": true },
    { "path": ["$.resident_city_unicode"], "optional": true },
    { "path": ["$.resident_state"], "optional": true },
    { "path": ["$.resident_postal_code"], "optional": true },
    { "path": ["$.resident_country"], "optional": true },
    { "path": ["$.nationality"], "optional": true }
  ]
}
```

---

## Appendix 2: Available Attributes

These are the available attributes from account-based integration partners.
| Field | Description | Connect ID | Select ID |
| :--- | :--- | :---: | :---: |
| **given_name_unicode** | First name of the holder, in Unicode. | ✅ | ✅ |
| **family_name_unicode** | Last name of the holder, in Unicode. | ✅ | ✅ |
| **birth_date** | ISO 8601 date of birth. | ✅ | ✅ |
| **document_number** | Document or passport number. | ❌ | ✅ |
| **issuing_country** | Country code of issuing authority. | ❌ | ✅ |
| **expiry_date** | Document expiry date. | ❌ | ✅ |
| **portrait** | Base64-encoded portrait image. | ❌ | ✅ |
| **resident_address_unicode** | Full address in Unicode. | ✅ | ✅ |
| **age_over_18** | Boolean indicating age eligibility. | ✅ | ❌ |
| **phone_number** | Phone number if collected. | ✅ | ✅ |
| **email** | Email address if provided. | ✅ | ✅ |
