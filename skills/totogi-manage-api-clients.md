---
name: Manage Totogi API client credentials
description: Mint, list, rotate and revoke the OAuth 2.0 client credentials that authenticate machine access to Totogi Charging-as-a-Service.
api: graphql/totogi-charging-as-a-service.graphql
endpoint: https://gql.produseast1.api.totogi.com/graphql
operations: [createClientCredentials, getClientCredentials, listAllClientCredentials, updateClientCredentials, deleteClientCredentials]
roles: [Tenant_Admin]
generated: '2026-07-25'
method: generated
source: https://docs.api.totogi.com/
---

# Manage Totogi API client credentials

Totogi's credential lifecycle is itself an API. Every operation here requires the
**`Tenant_Admin`** role — it is the highest-privilege surface in the schema and should be held by
a separate token from your day-to-day integration token.

## How the token flow works

1. A `Tenant_Admin` mints a client (key + secret) with `createClientCredentials`.
2. The integration exchanges that key/secret for an access token:
   POST `grant_type=client_credentials` to `https://oauth.totogi.io/oauth2/token` with the
   key:secret Basic-encoded in the `Authorization` header.
3. The returned access token goes in the `Authorization` header of every GraphQL request.
4. What that token may call is decided by the **named roles** attached to the client. Roles are
   published per operation in the reference and catalogued in `scopes/totogi-scopes.yml`.

## Steps

1. **Create** — `createClientCredentials(providerId: ID!, input: CreateClientCredentialsInput!): CreateClientCredentialsResult!`
   Capture the secret at creation; assume you cannot read it back.
   `ClientCreationLimitExceeded` carries `currentClientCount` and `maxClientLimit` — the tenant
   has a hard cap on live clients, so revoke before you mint.

2. **Inventory** — `listAllClientCredentials(providerId: ID!): ListAllClientCredentialsResult!`
   and `getClientCredentials(providerId: ID!, clientId: ID!): GetClientCredentialsResult!`.
   Run the inventory before every mint and on a schedule; there is no other console-free way to
   see what is live.

3. **Rotate** — `updateClientCredentials(providerId: ID!, clientId: ID!, input: UpdateClientCredentialsInput!): UpdateClientCredentialsResult!`
   Rotate by minting the replacement first, cutting traffic over, then deleting the old client —
   Totogi publishes no grace period or overlap window, so overlap has to be engineered by you.

4. **Revoke** — `deleteClientCredentials(providerId: ID!, clientId: ID!): DeleteClientCredentialsResult!`

## Errors

- `ClientNotFound` — carries `providerId` and `clientId`.
- `ClientCreationLimitExceeded` — carries `currentClientCount`, `maxClientLimit`.
- `ProviderNotFound`, `InvalidProviderLifecycleStage`.
- `RateLimitExceeded` (back off to `retryAfter`), `InternalServerError`.

## Related user administration

Human users are a separate object graph with the same admin posture: `createUser`, `updateUser`,
`resetUserPassword`, `deleteUser`, `listUsers`, `getUser`, `getCurrentUser`, `updateUserProfile`.
`UserIsReadOnly` is returned when a user tries to modify their own record;
`UserIncorrectStatus` when the user has not completed verification.

Full catalog: `errors/totogi-error-catalog.yml`. Auth model: `authentication/totogi-authentication.yml`.
