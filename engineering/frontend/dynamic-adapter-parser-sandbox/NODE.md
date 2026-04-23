---
title: "Dynamic Adapter Parser Sandbox"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend", "adapters"]
---

# Dynamic Adapter Parser Sandbox

External adapters can ship a `dist/ui-parser.js` that the Paperclip UI fetches from `/api/adapters/:type/ui-parser.js` to turn stdout lines into `TranscriptEntry[]`. Because this code is third-party and runs in the browser, it is executed inside a **dedicated Web Worker sandbox** rather than evaluated on the main thread. The worker is spawned from an inline Blob URL — no extra file is served — and communicates with the UI through a narrow postMessage protocol.

## Security boundary

The worker bootstrap runs in strict mode and shadows dangerous globals with `undefined` or no-ops before evaluating the parser source. This explicitly disables network and escape-hatch APIs: `fetch`, `XMLHttpRequest`, `WebSocket`, `EventSource`, `importScripts`, `navigator.sendBeacon`, child `Worker`/`SharedWorker`, `Blob`, `RTCPeerConnection`/`RTCDataChannel`, and `URL.createObjectURL` / `revokeObjectURL`. The intent is that a malicious or buggy parser cannot reach the board UI's same-origin state (cookies, localStorage, DOM, authenticated fetch) nor open side channels out of the worker.

## Protocol and sync fast-path

Messages are: `init { source }` → `ready | error`, then `parse { id, line, ts }` → `result { id, entries }`. Because postMessage is async but the existing `parseStdoutLine` contract is synchronous, completed worker results are cached and the adapter registry is notified via `setDynamicParserResultNotifier` to recompute transcripts when new results arrive. In practice this adds roughly one frame of latency, which is imperceptible. The unused `parse_batch` branch is intentionally excluded from the bootstrap, and tests in `sandboxed-parser-worker.test.ts` assert the lockdown strings and strict-mode wrapper are present.
