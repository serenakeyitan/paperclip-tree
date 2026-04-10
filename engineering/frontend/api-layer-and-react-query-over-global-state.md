---
title: "API Layer And React Query Over Global State"
owners: [cryppadotta, serenakeyitan]
---

# API Layer And React Query Over Global State

Paperclip's UI needs to show a large, fast-changing control plane without
turning the frontend into a second backend or a tangle of ad hoc fetch logic.

## Decision

Centralize HTTP calls in a dedicated API layer and manage server state with
React Query plus domain hooks and context providers, instead of introducing a
global state library such as Redux or Zustand.

## Why

This keeps data fetching, invalidation, and loading/error handling close to the
actual server resources while keeping UI-only state separate. It matches the
shape of a control-plane product better than a single app-wide mutable store.

## Implications

- Components should not call `fetch` directly when a shared API client or hook
  belongs in `/ui/src/api` or `/ui/src/hooks`.
- Server-derived state lives in query caches; UI-only concerns live in React
  context or local component state.
- Backend contract changes should usually surface first in the API layer rather
  than being reimplemented independently across many pages.
- Adapter-specific management surfaces can remain co-located with adapter code
  while still participating in the same frontend data flow.

## Related Domains

- [engineering backend](../backend/NODE.md)
- [engineering shared](../shared/NODE.md)
- [adapters](../../adapters/NODE.md)
