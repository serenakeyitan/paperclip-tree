---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The adapters node documents runChildProcess() and the local adapter spawning pattern but does not mention Windows-specific shell handling — local adapters must use cmd.exe when the CLI binary is a .cmd/.bat wrapper, which is the common case for npm-installed CLIs on Windows.
---
### Windows Process Spawning

On Windows, many agent CLIs are installed via npm and exposed as `.cmd` or `.bat` wrappers. These cannot be spawned directly — they must be invoked through `cmd.exe` (i.e., `shell: true` or explicit `cmd.exe /c` invocation in Node's `child_process`). The shared `runChildProcess()` utility in adapter-utils handles this detection, so individual adapters do not need to special-case it.
