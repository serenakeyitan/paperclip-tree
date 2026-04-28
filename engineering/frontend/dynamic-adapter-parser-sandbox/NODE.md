---
title: "Dynamic Adapter UI Parser Sandbox"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, engineering/frontend/NODE.md, plugins/runtime/NODE.md]
---

External adapters can ship a `dist/ui-parser.js` that the Paperclip UI loads on demand from `/api/adapters/:type/ui-parser.js` to render their stdout transcripts. Because parser code originates outside the Paperclip codebase, it is **not** evaluated on the main thread. Instead it runs inside a dedicated Web Worker created from an inline Blob URL, isolating it from same-origin state (cookies, localStorage, DOM, authenticated `fetch`).

## Sandbox lockdown

The worker bootstrap (`ui/src/adapters/sandboxed-parser-worker.ts`) shadows dangerous globals with `undefined` or no-ops before evaluating the parser source in strict mode. Disabled surfaces include `fetch`, `XMLHttpRequest`, `WebSocket`, `EventSource`, `importScripts`, `navigator.sendBeacon`, child `Worker` / `SharedWorker`, `Blob`, `RTCPeerConnection` / `RTCDataChannel`, and `URL.createObjectURL` / `revokeObjectURL`. This denies both network exfiltration and escape hatches that could spawn an unsandboxed context.

## Async-to-sync bridge

The existing `parseStdoutLine` contract is synchronous, but worker postMessage is async. `dynamic-loader.ts` resolves this by caching completed worker results and asking the adapter registry to recompute transcripts when a new result arrives — `setDynamicParserResultNotifier(notifyAdapterChange)` wires the worker's result callback into the registry's change listeners. In practice this adds ~1 frame of latency, which is imperceptible.

## Protocol

A narrow postMessage protocol (`SandboxRequest` / `SandboxResponse`) carries `init` (parser source), `parse` (line + timestamp keyed by id), `ready`, `error`, and `result` messages. The protocol intentionally has no `parse_batch` branch — kept minimal to reduce attack surface.
