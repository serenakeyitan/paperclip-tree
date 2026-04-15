---
title: "Live Updates"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Live Updates

Real-time update infrastructure for keeping issue-related UI surfaces current as agents and humans make changes.

## Key Decisions

### React Context Provider Pattern

Live updates are delivered via a React context provider (`LiveUpdatesProvider`) that wraps issue-related views. This integrates with React Query's cache invalidation rather than introducing a separate synchronization layer, keeping the architecture aligned with the existing data-fetching patterns.

### Optimized for Agent Activity

The live update system is designed to handle the high-frequency mutations typical of autonomous agent activity — rapid status transitions, comment bursts, and concurrent field updates — without overwhelming the UI with refetches.

### Breadcrumb-Aware Context

The `BreadcrumbContext` tracks the user's current navigation position (which issue, which project) and is consumed by the live update layer to scope invalidation. This prevents unrelated cache busts and keeps update traffic proportional to what the user is viewing.

## Boundaries

- Transport mechanism (SSE, WebSocket, polling) is a backend concern — the frontend consumes invalidation signals through the provider.
- The provider wraps issue-related views; non-issue surfaces are not covered.
