---
title: "Windows Shell Invocation"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Windows Shell Invocation

All local adapters must use `cmd.exe /c` (or `shell: true`) to invoke `.cmd`/`.bat` CLI shims on Windows. This is required because npm-installed CLI tools on Windows are `.cmd` batch scripts, not direct executables — spawning them directly via `child_process.spawn()` fails with `ENOENT`.

**Source:** `packages/adapter-utils/` (`runChildProcess`)

---

## Key Decision

**Centralize Windows-aware invocation in `adapter-utils/runChildProcess`** rather than duplicating the fix in each adapter. All local adapters (`claude-local`, `codex-local`, `cursor-local`, `gemini-local`, `opencode-local`, `pi-local`) inherit correct Windows behavior through this shared utility.

The pattern: detect `.cmd`/`.bat` extension on the target binary and prepend `cmd.exe /c` before spawning, or use Node's `shell: true` option. This ensures consistent cross-platform behavior without per-adapter platform checks.

## Boundaries

- **Gateway adapters** (`*-gateway`) are unaffected — they connect over WebSocket, not local process spawn.
- **macOS and Linux** are unaffected — CLI shims are regular executables on these platforms.
- This constraint applies to any future local adapter that spawns a CLI via `runChildProcess`.
