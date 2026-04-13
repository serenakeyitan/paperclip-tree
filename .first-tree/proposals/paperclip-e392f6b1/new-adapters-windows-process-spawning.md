---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree documents runChildProcess() but has no node covering Windows-specific shell behavior — the PR introduces a platform-aware pattern where .cmd/.bat files must be invoked via cmd.exe on Windows, which affects all local adapters.
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
