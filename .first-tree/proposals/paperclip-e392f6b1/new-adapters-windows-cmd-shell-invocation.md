---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree documents adapter process spawning via runChildProcess() but captures no cross-platform constraints; Windows .cmd/.bat files requiring cmd.exe shell invocation is an architectural constraint future adapter contributors need to know.
---
# Windows .cmd/.bat Shell Invocation

On Windows, many agent CLIs are distributed as `.cmd` or `.bat` batch files (e.g., `codex.cmd`). Node.js's `child_process.spawn` does not natively handle these — they must be invoked through `cmd.exe` as the shell.

## Decision

The shared `runChildProcess()` utility in `adapter-utils` detects when the target executable ends in `.cmd` or `.bat` and automatically routes the spawn through `cmd.exe` (`shell: 'cmd.exe'`). Individual adapters do not need to handle this — it is a shared concern.

## Why This Matters

- Without this, all local adapters silently fail on Windows 11 when the agent CLI is installed as a `.cmd` wrapper (common for npm-global installs).
- This is a Node.js platform behavior, not a Paperclip design choice — `spawn()` without `shell: true` cannot execute `.cmd` files on Windows.
- The fix is intentionally narrow: only `.cmd`/`.bat` extensions trigger the `cmd.exe` shell path. All other executables continue to spawn directly to avoid shell injection surface area.
