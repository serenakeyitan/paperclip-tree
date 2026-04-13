---
title: "Windows Process Spawning"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Windows Process Spawning

On Windows, many CLI tools (including Node.js-based agents) are installed as `.cmd` or `.bat` shims. Node's `child_process.spawn()` does not automatically use `cmd.exe` to invoke these, which causes silent failures or `ENOENT` errors on Windows 11+.

**Source:** `packages/adapter-utils/` — shared `runChildProcess()` utility.

---

## Key Decision

All local adapters use `cmd.exe /c` as the shell when spawning `.cmd` or `.bat` executables on Windows. This is handled in the shared `runChildProcess()` layer in adapter-utils, so individual adapters do not need platform-specific logic.

The fix specifically targets Windows `.cmd`/`.bat` shims — on Unix-like systems, the standard spawn behavior is unchanged.

---

## Boundaries

- This is a process-spawning concern only. It does not affect gateway adapters (which use WebSocket, not child processes).
- Individual adapter packages should not implement their own Windows shell handling — it belongs in adapter-utils.
