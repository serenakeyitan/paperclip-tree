---
title: "OpenClaw Gateway Adapter"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend"]
---

# OpenClaw Gateway Adapter

Adapter for **OpenClaw** agents via the WebSocket gateway protocol. Unlike all other adapters, this is a **gateway adapter** -- it connects to a remote agent over WebSocket rather than spawning a local CLI process.

**Source:** `packages/adapters/openclaw-gateway/`
**Package:** `@paperclipai/adapter-openclaw-gateway`
**Adapter type key:** `openclaw_gateway`

---

## Capabilities

- **Transport:** WebSocket only (`ws://` or `wss://`). Follows a structured protocol: challenge, connect, agent request, wait, event streaming.
- **Session resume:** Yes, via session key strategies (`issue`, `fixed`, `run`).
- **Auth modes:** Token-based (`authToken`, `x-openclaw-token`), legacy header (`x-openclaw-auth`), or shared password.
- **Device auth:** Ed25519 signed device identity with optional auto-pairing on first connect.
- **Environment test:** Validates gateway URL reachability and auth configuration.

---

## Key Decisions

- **WebSocket gateway protocol** instead of HTTP REST. This enables real-time bidirectional event streaming and eliminates polling. The protocol uses typed frames (`req`, `res`, `event`) with correlation IDs.
- **Device auth with auto-pairing** (`autoPairOnFirstConnect`) allows zero-config setup for new devices. The adapter handles the initial pairing flow transparently, calling `device.pair.list` + `device.pair.approve` over shared auth, then retries.
- **Ephemeral Ed25519 keypair** generated per-run by default. A stable key can be pinned via `devicePrivateKeyPem` config for persistent device identity.
- **Session key strategy** (`issue`/`fixed`/`run`) gives operators flexibility in how conversations are routed. `issue` ties sessions to Paperclip issues, `fixed` uses a static key, `run` creates a new session per execution.
- **No skill sync.** OpenClaw agents manage their own capabilities remotely. Paperclip does not inject skills into gateway-mode agents.
- **No session codec exported.** Unlike local adapters, session state is managed server-side by the OpenClaw gateway.

## Decision Records

- [websocket-gateway-and-device-pairing.md](websocket-gateway-and-device-pairing.md) — Why OpenClaw is modeled as a gateway protocol with pairing and remote session routing.

---

## Boundaries

- Does not manage the OpenClaw server itself. Only connects as a client.
- No local CLI dependency. The `ws` package is the only transport dependency.
- No quota or model reporting. The remote agent owns those concerns.
