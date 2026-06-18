# Using the Wallet API

This page covers the basics every SVX Wallet API call shares: where requests go, how to authenticate, and the conventions the API follows. The task guides in this section build on it.

## Base URL

All requests are made against the base URL of your SVX Wallet deployment, for example `https://wallet.example.com`. The paths shown throughout this section (such as `POST /wallets`) are relative to that base URL.

## Authentication

Every request must include an access token as a `Bearer` token in the `Authorization` header:

```
Authorization: Bearer <access_token>
```

Access tokens are issued by the **identity provider (IdP) configured for your SVX Wallet deployment**. How you obtain a token depends on that provider; your administrator provides the IdP's token endpoint and the credentials to use with it.

Obtain a token from your configured IdP, then send it as the `Authorization: Bearer` header on every subsequent request. The token identifies your integration to the wallet (it carries an `application` role).

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
