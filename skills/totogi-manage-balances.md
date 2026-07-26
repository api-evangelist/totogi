---
name: Top up and adjust Totogi account balances idempotently
description: Create, update and remove balances on a Totogi Charging-as-a-Service account using transaction ids so retries never double-charge.
api: graphql/totogi-charging-as-a-service.graphql
endpoint: https://gql.produseast1.api.totogi.com/graphql
operations: [createBalance, updateBalance, deleteBalance, getAccount, getAccountBalanceTypeCounters]
roles: [Account_Admin, Account_Query]
generated: '2026-07-25'
method: generated
source: https://docs.api.totogi.com/
---

# Manage Totogi account balances

Money movement is the part of this API you must get idempotency right on. Totogi gives you an
explicit contract for it — use it.

## The idempotency contract

`CreateBalanceInput.transactionId` is an optional `ID`:

> "An optional transaction ID to ensure idempotency of the API call, allowing for transaction
> correlation between Totogi and upstream systems. If not provided, Totogi automatically
> generates a unique identifier for each transaction."

Rules to follow:

- **Always supply your own `transactionId`** derived from the upstream event you are reacting
  to. That makes the whole retry path safe end to end, not just inside Totogi.
- On a replay, the result union returns **`TransactionHasBeenProcessed`** (which carries
  `providerId` and `transactionId`). Treat that as **success, already applied** — not a failure.
- `BalancePayload.transactionId` comes back on the success path. Persist it; it is the
  correlation key between your ledger and Totogi's.
- The same `transactionId` field is present on account mutations too (`createAccount`,
  `updateAccount`, `deleteAccount`), so the same discipline applies there.

## Steps

1. **Add a balance** — `createBalance(input: CreateBalanceInput!): CreateBalanceResult`
   Supply either `balanceInfo` or `balanceInfos`, never both — supplying both raises
   `InvalidField(CannotProvideBothBalanceInfoAndBalanceInfos)`, supplying neither raises
   `InvalidField(MissingBalanceInfoInput)`.
   You cannot add balances to the default balance type
   (`InvalidField(CreatingBalanceInDefaultBalanceType)`); for the default monetary balance type
   only `value` may be provided.

2. **Adjust a balance** — `updateBalance(input: UpdateBalanceInput!): UpdateBalanceResult`
   Adjustments are typed by `AdjustmentType`: `CREDIT` adds, `DEBIT` subtracts, `SET` replaces,
   `NONE` leaves the amount alone.

3. **Remove a balance** — `deleteBalance(input: DeleteBalanceInput!): DeleteBalanceResult`
   A balance referenced by an active account plan raises `BalanceHasReferences`.

4. **Verify** — `getAccount(providerId: ID!, accountId: ID!, balanceTypeIds: [ID!]): AccountResult`
   and read `balances`. Pass `balanceTypeIds` to narrow the read.
   For threshold counters use
   `getAccountBalanceTypeCounters(input: GetAccountBalanceTypeCountersInput!)`.

## Errors that matter here

- `TransactionHasBeenProcessed` — idempotent replay. Success.
- `AccountNotFound` (carries `providerId`, `accountId`), `BalanceTypeNotFound`, `BalanceNotFound`.
- `InvalidField(BalanceLimitViolation)` — the balance total would exceed the balance or
  balance-type limit, or the limit itself exceeds the balance type's limit.
- `InvalidField(InvalidLimit)` — limits must be integers unless the balance type is rate-based.
- `PolicyCounterNotFound` and `InvalidField(PolicyTypeNotAllowed)` — only `NOTIFICATION` type
  policy counters can be associated with balances.
- `RateLimitExceeded` — carries `retryAfter` (`AWSDateTime`). Back off to that timestamp; do not
  retry blindly, and reuse the same `transactionId` when you do.

Full catalog: `errors/totogi-error-catalog.yml`.
