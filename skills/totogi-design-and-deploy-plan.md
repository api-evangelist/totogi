---
name: Design, deploy and make a Totogi rate plan assignable
description: Take a Totogi Charging-as-a-Service plan from creation through versioning, deployment and assignability so subscribers can be subscribed to it.
api: graphql/totogi-charging-as-a-service.graphql
endpoint: https://gql.produseast1.api.totogi.com/graphql
operations: [createPlan, createPlanFromInitialTemplate, createPlanVersionFromInitialTemplate, deployPlan, makePlanAssignable, getPlan, getPlans, getPlanVersion, archivePlanVersion]
roles: [Plan_Designer, Plan_Publisher, Plan_Query]
generated: '2026-07-25'
method: generated
source: https://docs.api.totogi.com/
---

# Design and deploy a Totogi rate plan

Plans in Totogi are versioned objects that move through a state machine. Two different roles own
two different halves of it: **`Plan_Designer`** creates and edits, **`Plan_Publisher`** deploys
and makes assignable. A single token usually cannot do the whole flow.

## Steps

1. **Create the plan.**
   - From scratch: `createPlan(providerId: ID!, input: PlanInput!): CreatePlanResult!`
     *(role: `Plan_Designer`)*
   - From a Totogi template:
     `createPlanFromInitialTemplate(input: CreatePlanFromInitialTemplateInput!)` or
     `createPlanFromInitialRecurringFirstUsageTemplate(...)` — the recurring/first-usage template
     is the one to reach for when the plan bills on a cycle that starts at first use.

2. **Add a version.**
   `createPlanVersionFromInitialTemplate(input: CreatePlanVersionFromInitialTemplateInput!)` or
   `createPlanVersionFromInitialRecurringFirstUsageTemplate(...)`. Edits go to versions, never to
   a deployed plan — a deployed version answers `PlanVersionIsReadOnly`.

3. **Deploy it.** `deployPlan(input: DeployPlanVersionInput!): DeployPlanVersionResult!`
   *(role: `Plan_Publisher`)*. Deployment runs verification; a failure returns
   `DeploymentVerificationFailed` carrying `providerId` and `planVersionId`.

4. **Make it assignable.**
   `makePlanAssignable(input: AssignablePlanVersionInput!, revert: Boolean): AssignablePlanVersionResult!`
   *(role: `Plan_Publisher`)*. Pass `revert: true` to take it back out of circulation.
   Until this succeeds, `subscribeToPlan` will return `PlanVersionIsNotAssignable`.

5. **Confirm.** `getPlanVersion(providerId: ID!, planVersionId: ID!): PlanVersionResult`, or list
   with `getPlans(providerId: ID!, first: Int!, after: String, orderBy: PlanOrder): PlanConnection`.
   `getPlans` is Relay cursor-paginated and `first` is **non-null** — you must always state a page
   size, then follow `pageInfo` with `after`.

6. **Retire a version.** `archivePlanVersion(...)`. Versions with live subscriptions return
   `PlanVersionHasReferences`.

## Errors that matter here

- `PlanAlreadyExists` (carries `planId`), `PlanNotFound`, `PlanVersionNotFound`,
  `PlanVersionAlreadyExists`.
- `PlanVersionWrongTransition` — you tried to move the version between states in an order the
  machine does not allow. Re-read the current state before retrying.
- `PlanVersionUpdateAlreadyInProgress`, `MigrationAlreadyInProgress` — a concurrent operation
  holds the version. Poll, do not hammer.
- `CreatePlanValidationFailed`, `PlanServiceValidationFailed`, `ServiceFormatError` — the plan
  body itself is not constructible.
- `PlanIsReadOnly` — the initial template plan cannot be edited; copy it to a new plan first.
- `BalanceTypeReferencesNotFound` — carries `invalidReferences`, the expression references a
  balance type that does not exist. Fix the expression, not the plan state.
- `QoSProfileNotFound`, `PolicyCounterNotFound` — referenced policy objects must exist first
  (`createQoSProfile`, `createPolicyCounter`).
- `InvalidProviderLifecycleStage`, `RateLimitExceeded` (back off to `retryAfter`),
  `InternalServerError`.

Full catalog: `errors/totogi-error-catalog.yml`. Conventions: `conventions/totogi-conventions.yml`.
