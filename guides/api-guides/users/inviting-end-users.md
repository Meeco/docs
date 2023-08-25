# Inviting End-Users to a Tenant

The sequence diagram below shows the process of how a user controlling a DID is invited and then added to our system by a tenant administrator.

```mermaid
sequenceDiagram
  title Invite End-User using Wallet to Tenancy

  autonumber

  actor H as Holder
  actor TA as Tenant Admin

  participant P as Portal
  participant W as Wallet
  participant API as SVX API

  W->>W: did:key generated locally
  TA->>P: Navigate to Add User > End Users > Manage Tenancy.
  P->>+API: POST /end-users/invitations
  API-->>-P: 201 invitation object containing request_uri
  Note over H,P: There are different ways a holder can receive this link<br>- QR code that can be scanned<br>-Deeplink to click on
  P->>P: Render QR for request <br> openid://request_uri=https://svx-api-sandbox.meeco.me/oidc/presentations/requests/{id}/jwt

  TA-->>H: Email QR code / deeplink

  H->>W: Scan QR Code
  W->>+API: GET https://svx-api-sandbox.meeco.me/oidc/presentations/requests/{id}/jwt
  API-->>-W: return request_jwt
  W->>+W: Extract state and reciret_uri attributes from the request request_jwt
  W->>+API: POST /end_users/invitations/short_lived_access_token (token: state)
  API-->>-W: return short lived access_token
  W->>+API: POST /oidc/presentations/requests/verify (request_jwt, short lived access_token)
  API-->>-W: 201 OK
  W->>+API: POST /oidc/presentations/token<br>(request_id, state, short lived access_token)
  API-->>-W: 201 OK and unsigned id_token
  W->>W: Sign id_token

  W->>+API: POST /end_users/invitations/{token}/accept <br> (id_token, state: token, short lived access_token) <br> (endpoint is defined under redirect_uri inside the request_jwt)

  API-->>-W: 201 OK (access_token: {access_token})
```