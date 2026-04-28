---
title: "Environments & Leases"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/database/NODE.md, engineering/execution-workspaces/NODE.md, engineering/shared/NODE.md, product/company-model/NODE.md]
---

# Environments & Leases

Paperclip models the place where an agent run actually executes as a first-class **environment**. An environment is company-scoped, has a `driver` (initially only `local`), a `status` (`active` | `archived`), and free-form `config` / `metadata` JSON. Each company has at most one environment per driver (enforced by a unique `(company_id, driver)` index), and the system seeds a default `Local` environment named "Local" the first time a heartbeat run needs one.

When a run needs to use an environment, the backend acquires an **environment lease** rather than touching the environment row directly. A lease ties an environment to the consumer that borrowed it — typically an `execution_workspace_id`, `issue_id`, and/or `heartbeat_run_id` — and tracks `status` (`active` | `released` | `expired` | `failed`), `lease_policy` (currently only `ephemeral`), optional `provider` / `provider_lease_id` for external drivers, timing fields (`acquired_at`, `last_used_at`, `expires_at`, `released_at`), a `failure_reason`, and a `cleanup_status` (`pending` | `success` | `failed`). Leases are the unit of accountability: a run that fails or is cancelled releases its lease, and cleanup status records whether the underlying resources were torn down.

The lifecycle is owned by `environmentService` (`server/src/services/environments.ts`) and integrated into the heartbeat run path (`server/src/services/heartbeat.ts`): heartbeat acquires a lease before launching a run on a local environment and releases it when the run finishes. This isolates run accounting from workspace plumbing and gives a single place to add non-local drivers later.

## Why this exists

- Decouple *where* a run executes from the workspace/agent records, so future drivers (remote sandboxes, cloud VMs, etc.) can plug in by adding a new `driver` value plus a provider implementation without reshaping callers.
- Make resource accountability explicit: every run that consumes an environment leaves a lease row that records who held it, when, and how cleanup went.
- Keep company isolation strict — environments and leases both carry `company_id` with `ON DELETE CASCADE`, matching the rest of Paperclip's company-scoped model.

## Current scope

- Only the `local` driver is implemented; `ENVIRONMENT_DRIVERS = ["local"]` and `ENVIRONMENT_LEASE_POLICIES = ["ephemeral"]` are intentionally one-element enums to leave room for expansion.
- Schema lives in `packages/db/src/schema/environments.ts` and `environment_leases.ts` (migration `0065_environments.sql`); shared types and Zod validators live under `packages/shared/src/types/environment.ts` and `packages/shared/src/validators/environment.ts`.
- Heartbeat is currently the only consumer; UI/API surface for managing environments is not part of this PR.
