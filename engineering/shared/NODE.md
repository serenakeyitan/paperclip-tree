---
title: "Shared Package"
owners: [cryppadotta, serenakeyitan]
soft_links: ["engineering/backend", "engineering/frontend", "engineering/cli", "engineering/database"]
---

# Shared Package

Cross-package types, constants, validators, and configuration schemas. Source: `/packages/shared`.

Package: `@paperclipai/shared`

## Purpose

This package is the single source of truth for domain vocabulary shared across backend, frontend, CLI, and adapter packages. It ensures consistent type definitions and validation rules without circular dependencies.

## Contents

### Constants & Enums
Exhaustive const arrays for every domain status, type, and category — used for both TypeScript types and runtime validation:
- Company statuses, deployment modes, deployment exposures
- Agent statuses, adapter types, roles, icon names
- Issue statuses, priorities, origins, execution policies, execution states
- Goal levels, goal statuses
- Project statuses, project colors
- Routine statuses, concurrency policies, trigger kinds, run statuses
- Approval types and statuses
- Budget scopes, metrics, window kinds, threshold types, incident statuses
- Finance event kinds, directions, units, billing types
- Secret providers, storage providers
- Heartbeat invocation sources, run statuses

### Type Definitions (`/src/types/`)
TypeScript interfaces and type aliases for API request/response shapes, domain entities, and configuration objects.

### Validators (`/src/validators/`)
Zod or similar schemas for runtime validation of API inputs, config files, and adapter payloads.

### Config Schema (`/src/config-schema.ts`)
Shared configuration shape used by both server and CLI.

### Utilities
- `routine-variables.ts` — template variable interpolation for routine definitions
- `project-mentions.ts` — parsing @-mentions in project descriptions
- `agent-url-key.ts` / `project-url-key.ts` — URL slug generation
- `execution-workspace-guards.ts` — workspace state validation helpers

## Key Decisions

- **Const arrays, not TypeScript enums.** All domain values are `as const` arrays, which enables both type inference (`typeof X[number]`) and runtime iteration/validation. TypeScript enums are avoided.
- **No runtime dependencies.** This package has zero npm dependencies — it is pure TypeScript types and constants. This keeps it safe to import anywhere without bloating bundles.
- **Telemetry types are co-located** (`/src/telemetry/`) so both server and CLI share the same event schemas.
