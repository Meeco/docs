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
> The `client_id` and `client_secret` come from an **API key** registered in the SVX Wallet Dashboard. If you have access to the Dashboard you can register one yourself; otherwise an administrator provides the credentials. The token is scoped to the deployment it was issued for, so use a token obtained for the deployment you are calling.

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
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiI…",
  "token_type": "Bearer",
  "expires_in": 900
}
```

Send the returned `access_token` as the `Authorization: Bearer` header on every subsequent request. When a token expires (`expires_in` is its lifetime in seconds), requests are rejected with `401 Unauthorized`; repeat the exchange to obtain a new one.

> **Note**
> Some deployments issue tokens through an external identity provider instead of the built-in endpoint above. In that case, obtain a token from your deployment's configured provider — your administrator provides the details — and use it the same way.

Tokens are short-lived. When a token expires, requests are rejected with `401 Unauthorized`; obtain a new token from the IdP and retry.

## Request conventions

* **JSON** — request and response bodies are JSON. Send `Content-Type: application/json` on requests with a body.
* **Identifiers** — `walletId` and similar path parameters are UUIDs, as returned when the resource is created.
* **Status codes** — successful reads and updates return `200`, resource creation returns `201`, and successful deletes return `204 No Content`. Errors use standard `4xx`/`5xx` codes; an expired or missing token returns `401`.

## Next steps

Once you can authenticate, follow the task guides:

* [Manage Wallets and Keys](manage-wallets.md) — create a wallet and the keys it holds.
* [Add Credentials to a Wallet](add-credentials.md) — receive or import credentials.
* [Present Credentials](present-credentials.md) — present credentials to a verifier.
