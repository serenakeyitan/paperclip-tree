---
title: "Terminal-Result Process Group Cleanup"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/capability-flags/NODE.md, adapters/claude-local/NODE.md, engineering/backend/NODE.md]
---

Adapters that spawn local CLI agents sometimes leave behind orphaned descendants in the spawned process group after the agent emits its terminal result and the parent child exits. To prevent these lingering process groups from holding workspace resources, `runChildProcess` in `@paperclipai/adapter-utils` now accepts an optional `terminalResultCleanup` option.

**Source:** `packages/adapter-utils/src/server-utils.ts`, `packages/adapters/claude-local/src/server/execute.ts`

## How it works

Callers pass `terminalResultCleanup: { hasTerminalResult, graceMs }` to `runChildProcess`. The runtime scans accumulated stdout/stderr (with a 64 KB overlap window, `TERMINAL_RESULT_SCAN_OVERLAP_CHARS`) for the adapter-specific terminal-result marker. Once `hasTerminalResult` returns true and the primary child has exited, the runtime waits `graceMs` (default 5000 ms) for the process group to drain on its own, then issues a group signal to clean up any survivors.

The scan-overlap window ensures the marker is detected even when it spans chunk boundaries in the streamed output.

## Adapter integration

Each adapter supplies its own `hasTerminalResult` predicate. The Claude local adapter uses `parseClaudeStreamJson(stdout).resultJson !== null` to detect Claude Code's terminal result event, and exposes a `terminalResultCleanupGraceMs` config field (default 5000 ms) so operators can tune the grace window per agent.

Other local adapters (Codex, Cursor, Gemini, OpenCode, Pi, Hermes) can opt in by providing their own terminal-result predicate; until they do, they fall back to the previous behavior with no terminal-result-aware cleanup.
