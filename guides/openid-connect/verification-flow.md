# Verifier - Holder Presentation Flow (OIDC4VP)

The sequence diagram below shows the process of how a verifier creates a verification request, sends it to a wallet holder, which subsequently verifies the request and submits a response.

```mermaid
sequenceDiagram
  title Verify Credentials with API only
  autonumber

  participant W as Wallet
  participant V as Verifier System
  participant API as SVX API

  %%
  %% Terms:
  %% VP - Verifiable presentation
  %% VP token - wrapping structure over a verifiable presentation.
  %%

  opt RESTful-API call to fetch details of organisation
    %% Get the DID for the organisation
    V-->>+API: GET /me MEECO_ORGANISATION_ID={org_id)
    API-->-V: 200 OK and me object (did_external_id, private_dek_external_id)
    %% Get the private key associated with DID for the organisation
    V->>+API: GET /keypairs/{did_external_id}
    API-->>-V: 200 OK and did signing keypair
  end

  %% Create presentation definition
  opt RESTful-API call to create or retrieve presentation definition
    V->>+API: POST /presentation_definitions
    API-->>-V: 200 and presentation definitions created

    V->>+API: GET /presentation_definitions/{id}
    API-->>-V: 200 and presentation definitions created
  end
  %% Create a presentation request
  V->>+API: POST /oidc/presentations/request
  API-->>-V: 201 OK and OIDC request string
  %% Sign request JWT
  V->>V: Sign request JWT with did signing keypair
  %% Update the request
  V-->>+API: PUT /oidc/presentations/requests/{id}
  API-->-V: 201 OK and presentation request object<br>(signed request jwt)

  V->>W: Show Presentation request as a QR-code or DL
  W->>W: Scan QR-code, or<br>open DL
  W->>API: GET /oidc/presentations/requests/{id}/JWT (public endpoint)
  API-->>W: 200 OK
  opt verifiy request signature
    W->>API: verify OIDC presentation request
    API-->>W: 200 OK
  end

  W->>W: Select credentials

  %% Build verifiable presentation
  W->>V: POST /presentations/generate [VC]
  V-->>W: return unsigned verifiable presentation (VP)
  W->>W: Sign VP

  %% Generate verifiable presentation in a form of VP token
  W->>API: POST /oidc/presentations/token
  API-->>W: 201 OK and unsigned VP token
  W->>W: Sign VP token

  %% Submit VP token to the redirect_uri
  W->>+V: POST "presentation_request.redirect_uri"

  %% Validate VP token and its content
  V->>API: POST /oidc/presentations/verify
  API->>API: Verify SIOP token signature and <br /> extract the verifiable presentation
  API->>API: Verify verifiable presentation structure, signatures and <br /> if data provided match presentation definition
  API-->>V: 204 OK / 201 OK and parsed token information

  alt Verify presentation
    Note right of V: This endpoint will check everything <br /> (signatures, structure, status list information) <br /> and might be an expensive call.
    V->>API: POST /presentations/verify
    API-->>V: 204 OK
  end

  V-->>W: 201 Submission is valid
```