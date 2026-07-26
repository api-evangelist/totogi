---
name: Page through Totogi event data records (EDRs)
description: Read charging event data records for an account or a device using Totogi's Relay cursor pagination, with correct backoff on rate limiting.
api: graphql/totogi-charging-as-a-service.graphql
endpoint: https://gql.produseast1.api.totogi.com/graphql
operations: [getEventDataRecordsByAccount, getEventDataRecordsByDevice, exportCustomerDB, getMyRestoreJobs]
roles: [Data_Admin, Product_Admin, System_Admin]
generated: '2026-07-25'
method: generated
source: https://docs.api.totogi.com/
---

# Page through Totogi event data records

EDRs are the usage record behind every charge. Reading them requires the **`Data_Admin`** role —
the account and plan roles will not open this door.

## Steps

1. **Choose the axis.**
   - By account:
     `getEventDataRecordsByAccount(providerId: ID!, accountId: ID!, first: Int!, after: String, filterBy: EventDataRecordFilter): EventDataRecordAccountConnectionResult`
   - By device:
     `getEventDataRecordsByDevice(providerId: ID!, deviceId: ID!, first: Int!, after: String, filterBy: EventDataRecordFilter): EventDataRecordDeviceConnectionResult`

2. **Page correctly.** Both are Relay cursor connections:
   - `first` is **`Int!`** — always state a page size; there is no server default to fall back on.
   - Read `pageInfo` off the connection, then pass its end cursor back as `after` on the next call.
   - Never assume offset semantics. There is no page number and no total count.

3. **Filter server-side** with `filterBy: EventDataRecordFilter` rather than pulling pages and
   discarding rows — every page you do not need is a page against the tenant rate limit.

4. **For bulk, do not page at all.** `exportCustomerDB(providerId: ID!, exportTime: AWSDateTime!)`
   *(roles: `Product_Admin`, `System_Admin`)* drops customer data into the tenant's own AWS
   account — this is the mechanism the 13 May 2025 "Customer DB Exports" release added. Track it
   with `getMyRestoreJobs`. Errors: `InvalidExportTime`, `ExportAlreadyRunning`.

## Rate limiting

`RateLimitExceeded` is a member of 70 of the 87 result unions on this API, including both EDR
queries. It carries **`retryAfter: AWSDateTime!`**. Handle it as data, not as an exception:

```graphql
... on RateLimitExceeded { errorCode errorMessage retryAfter }
```

Sleep until `retryAfter` before issuing the next page request, and resume from the **same
cursor** — do not restart the walk. Totogi publishes no numeric quota, so `retryAfter` is the
only limit signal you get.

## Other errors

`AccountNotFound`, `DeviceNotFound`, `ProviderNotFound`, `InvalidProviderLifecycleStage`,
`FeatureNotEnabled` (carries `featureName` — the capability is not switched on for this tenant),
`InternalServerError`.

Full catalog: `errors/totogi-error-catalog.yml`.
