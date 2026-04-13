---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Windows shell invocation convention (cmd.exe /c for .cmd/.bat shims) is a cross-adapter requirement not captured in any adapter node.
---
# Windows Shell Invocation

All local adapters must use `cmd.exe /c` (or `shell: true`) to invoke `.cmd`/`.bat` CLI shims on Windows. This is required because npm-installed CLI tools on Windows are `.cmd` batch scripts, not direct executables.

## Key Decisions

### Centralized in adapter-utils

The Windows-aware invocation logic should live in `adapter-utils/runChildProcess` rather than being duplicated in each adapter. This ensures consistent behavior and a single fix point if the Windows invocation strategy changes.

### Cross-Adapter Requirement

This applies to all local adapters (claude-local, codex-local, cursor-local, gemini-local, opencode-local, pi-local) — any adapter that spawns a CLI as a child process.
