---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The adapters node documents the Local vs Gateway pattern and runChildProcess but says nothing about Windows-specific process spawning — the PR reveals that .cmd/.bat files require cmd.exe as the shell on Windows, which is a cross-adapter constraint future contributors need to know.
---
Under **Local vs Gateway Pattern** or **Key Decisions**, add:

- **Windows `.cmd`/`.bat` shell handling:** On Windows, CLI tools installed via npm (e.g., `opencode.cmd`) are `.cmd`/`.bat` wrappers. Node's `child_process.spawn` cannot execute these directly — they must be invoked through `cmd.exe` (i.e., `shell: true` or explicit `cmd.exe /c` invocation). The shared `runChildProcess` utility handles this transparently so individual adapters don't need platform-specific spawning logic.
