---
title: "External Adapter Registration Resolution"
owners: [bingran-you, cryppadotta, serenakeyitan, superbiche]
soft_links: [adapters/NODE.md, adapters/capability-flags/NODE.md, engineering/backend/NODE.md]
---

External (non-builtin) adapters can enter the server registry through two paths: the init-time IIFE that loads adapter plugins on server start, and the hot-install path via `POST /api/adapters/install` (`registerWithSessionManagement` in `server/src/routes/adapters.ts`). Both paths must produce identical registry entries so an adapter installed at runtime has the same shape as the same adapter loaded on the next restart.

## Decision

A single shared function, `resolveExternalAdapterRegistration` in `server/src/adapters/registry.ts`, is the canonical resolver for external adapter registration. It honors module-provided `sessionManagement` first, then falls back to the host registry by type (so external adapters that override a builtin inherit the builtin's policy). If neither is available, `sessionManagement` remains `undefined`.

The hot-install path no longer wraps the adapter inline with `getAdapterSessionManagement(adapter.type)` — that logic discarded module-provided `sessionManagement`. Instead, it calls `resolveExternalAdapterRegistration` so both load paths stay at parity.

## Why

The previous hot-install wrapper unconditionally overwrote `sessionManagement` with the host registry lookup, dropping any session management config the adapter module declared itself. After a server restart, the init-time IIFE would resolve it differently, producing divergent registry entries for the same adapter depending on how it was loaded.
