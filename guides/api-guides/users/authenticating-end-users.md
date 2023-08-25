# Authenticating End-Users to a Tenant

The sequence diagram below shows the process of how a user controlling a DID, that is registered with a tenant, can authenticate to our system using a [digital wallet](/concepts/digital-wallets.md). This results in an access token that can be used to communicate with the SVX API.

This process is also referred to as DID authentication using SIOP V2.

```mermaid
sequenceDiagram
  title Authenticate End-User to Tenant using Wallet

  autonumber

  actor H as Holder

  participant W as Wallet
  participant API as SVX API

  H->>W: Open wallet
  W->>API: POST /user_authorisation/authentication_requests
  API-->>W: 201 openid://request_uri={request_uri}
  W->>API: GET {request_uri}
  API-->>W: return request_jwt
  Note over W, API: Wallet uses short lived bearer token to call APIs
  W->>API: POST /oidc/presentations/requests/verify (request_jwt)
  API-->>W: 200 OK
  W->>API: POST /oidc/presentations/token<br>(request_id, state)
  API-->>W: 201 OK and unsigned id_token
  W->>W: Sign id_token
  W-->>API: POST /user_authorisation/siop_sessions <br>(id_token, nonce)

  API-->>W: 201 OK (access_token: {access_token})
```