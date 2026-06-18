# Using the Wallet API

This page covers the basics every SVX Wallet API call shares: where requests go, how to authenticate, and the conventions the API follows. The task guides in this section build on it.

## Base URL

All requests are made against the base URL of your SVX Wallet deployment, for example `https://wallet.example.com`. The paths shown throughout this section (such as `POST /wallets`) are relative to that base URL.

## Authentication

Every request must include an access token as a `Bearer` token in the `Authorization` header:

```
Authorization: Bearer <access_token>
```

Tokens are obtained with a **client credentials** exchange using an application's `client_id` and `client_secret`.

> **Note**
> The `client_id` and `client_secret` identify an *application* registered with the deployment. An administrator registers the application in the SVX Wallet Dashboard and provides you with its `client_id` and `client_secret`. The secret is shown only once, so store it securely.

### Get an access token

Exchange your application credentials for an access token.

**Endpoint**

```bash
POST /application/token
```

**Request**

```json
{
  "grant_type": "client_credentials",
  "client_id": "<client_id>",
  "client_secret": "<client_secret>"
}
```

**Response**

```json
{
  "access_token": "eyJhbGciOiJFUzI1NiIsInR5cCI6ImF0K2p3dC…",
  "token_type": "Bearer",
  "expires_in": 900
}
```

Send the returned `access_token` as the `Authorization: Bearer` header on every subsequent request.

### Token expiry

`expires_in` is the token's lifetime in seconds (15 minutes). When a token expires, requests are rejected with `401 Unauthorized`; obtain a new token by repeating the exchange. There is no refresh token — request a fresh token with the same credentials.

## Request conventions

* **JSON** — request and response bodies are JSON. Send `Content-Type: application/json` on requests with a body.
* **Identifiers** — `walletId` and similar path parameters are UUIDs, as returned when the resource is created.
* **Status codes** — successful reads and updates return `200`, resource creation returns `201`, and successful deletes return `204 No Content`. Errors use standard `4xx`/`5xx` codes; an expired or missing token returns `401`.

## Next steps

Once you can authenticate, follow the task guides:

* [Manage Wallets and Keys](manage-wallets.md) — create a wallet and the keys it holds.
* [Add Credentials to a Wallet](add-credentials.md) — receive or import credentials.
* [Present Credentials](present-credentials.md) — present credentials to a verifier.
