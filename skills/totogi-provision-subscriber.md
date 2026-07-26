---
name: Provision a Totogi subscriber and put them on a plan
description: Create an account, attach a device, subscribe the account to a deployed plan version, and read the result back — the core provisioning flow of Totogi Charging-as-a-Service.
api: graphql/totogi-charging-as-a-service.graphql
endpoint: https://gql.produseast1.api.totogi.com/graphql
operations: [createAccount, createDevice, subscribeToPlan, getAccount, getDevice]
roles: [Account_Admin, Account_Query]
generated: '2026-07-25'
method: generated
source: https://docs.api.totogi.com/
---

# Provision a Totogi subscriber

Totogi Charging-as-a-Service is a GraphQL API. Every operation below is a real query or
mutation in `graphql/totogi-charging-as-a-service.graphql`; do not invent operation names,
argument names or field names — read them from that schema.

## Before you start

- **Get a token.** POST `grant_type=client_credentials` to `https://oauth.totogi.io/oauth2/token`
  with the client key and secret Basic-encoded in the `Authorization` header. Send the returned
  access token as `Authorization` on every GraphQL request.
- **You need the `Account_Admin` role** for all three mutations here, and `Account_Query`
  (or `Account_Admin`) to read back. Roles are published per operation; see
  `scopes/totogi-scopes.yml`.
- **`providerId` is mandatory everywhere.** It is the tenant key and the partition key of the
  whole data model. Nothing is addressable without it.
- **Pick your region endpoint:** `gql.produseast1.api.totogi.com` (US) or
  `gql.prodapsoutheast1.api.totogi.com` (Singapore).

## Steps

1. **Create the account** — `createAccount(input: CreateAccountInput!): CreateAccountResult!`
   Set `transactionId` on the input if you want the call to be idempotent. If you omit it,
   Totogi generates one and returns it so you can still correlate the transaction upstream.
   Replaying the same `transactionId` returns `TransactionHasBeenProcessed` rather than creating
   a second account.

2. **Create the device** — `createDevice(input: CreateDeviceInput!): CreateDeviceResult!`
   A device belongs to an account; the device id is the network-facing identifier the charging
   function will present.

3. **Subscribe the account to a plan version** —
   `subscribeToPlan(input: SubscribeToPlanVersionInput!): SubscribeToPlanVersionResult!`
   The plan version must already be deployed and assignable, or you will get
   `PlanVersionIsNotAssignable` / `PlanVersionNotFound`. See the
   `totogi-design-and-deploy-plan` skill for how a plan version reaches that state.
   A newly subscribed plan version stays **inactive** until the first credit or charge request —
   that is expected, not an error.

4. **Read it back** — `getAccount(providerId: ID!, accountId: ID!, balanceTypeIds: [ID!]): AccountResult`
   and `getDevice(providerId: ID!, deviceId: ID!): DeviceResult`.
   `Account` exposes `activePlanVersions`, `inactivePlanVersions`, `archivedPlanVersions` and
   `balances`, so one read confirms the whole provisioning outcome.

## Handling the response

Totogi returns **errors as data**. Every operation's return type is a union of the success
payload plus each typed error it can raise, delivered with HTTP 200. You must inline-fragment
on each member you handle:

```graphql
... on Account { ... }
... on AccountNotFound { errorCode errorMessage accountId }
... on RateLimitExceeded { errorCode retryAfter }
... on InternalServerError { errorCode errorMessage }
```

Errors you will hit on this flow specifically:

- `AccountAlreadyExists` — the `accountId` is taken.
- `InvalidField` — carries `fieldName`. Common causes on `createAccount`: an `accountId`
  containing `#`, `/` or `\` (reserved delimiters); a `timezone` outside `-12:00`..`+14:00`;
  a `billingDayOfMonth` outside 1–31; a `customProperties` key not declared in the provider's
  schema (`UndeclaredCustomProperty`) or of the wrong type (`CustomPropertyTypeMismatch`).
- `PostpaidPropertiesRequired` / `PostpaidFieldInPrepaidAccount` — `creditLimit` must be
  negative or zero for prepaid and positive for postpaid, and the postpaid property block must
  match.
- `InvalidProviderLifecycleStage` — the tenant itself is not in a state that permits the call.
- `RateLimitExceeded` — back off until the `retryAfter` timestamp it returns.

Full catalog: `errors/totogi-error-catalog.yml`. Cross-cutting rules: `conventions/totogi-conventions.yml`.
