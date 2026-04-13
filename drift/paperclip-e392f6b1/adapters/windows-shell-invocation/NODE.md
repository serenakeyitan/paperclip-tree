---
title: "Windows Shell Invocation for Local Adapters"
owners: []
---

# Windows Shell Invocation for Local Adapters

On Windows, agent CLIs installed via npm are exposed as `.cmd` or `.bat` shim files (e.g., `claude.cmd`, `opencode.cmd`). These cannot be spawned directly via `child_process.spawn()` — they must be invoked through `cmd.exe` using the `/c` flag, or spawned with the `shell: true` option.

This is a cross-cutting concern that affects **all local adapters** (`claude-local`, `codex-local`, `cursor-local`, `gemini-local`, `opencode-local`, `pi-local`). The fix was first applied in `opencode-local` (PR #2397) but the pattern applies to any adapter that uses `runChildProcess()` or direct `spawn()` to invoke an agent CLI.

## Key Decision

**Use `cmd.exe /c` (or `shell: true`) for `.cmd`/`.bat` invocation on Windows** rather than spawning the shim directly. Direct spawn fails silently or throws `ENOENT` on Windows because `.cmd` files are not executable binaries — they require the Windows command interpreter.

This logic should live in `adapter-utils` (`runChildProcess`) so that all local adapters inherit correct Windows behavior without per-adapter fixes.

## Boundaries

- Gateway adapters (`*-gateway`) are unaffected — they connect over WebSocket, not local process spawn.
- This constraint is Windows-specific; macOS and Linux invoke CLI shims as regular executables.
