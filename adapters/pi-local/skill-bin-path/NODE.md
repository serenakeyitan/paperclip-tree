---
title: "Pi Local Skill Bin PATH Injection"
owners: [bingran-you, cryppadotta, serenakeyitan, SgtPooki]
soft_links: [adapters/pi-local/NODE.md, plugins/runtime/NODE.md]
---

The pi_local adapter prepends each injected skill's `bin/` directory to the spawned Pi child process PATH. Without this, an agent's bash tool calling a skill command (e.g. `paperclip-get-issue`) by name fails with exit 127 "command not found".

## Scoping rule

Only skills that `ensurePiSkillsInjected` actually linked are added to PATH. Non-injected skills' binaries must remain unreachable to the agent — the filter uses the `desiredPiSkillNames` set against the resolved skill entries.

## PATH key handling

The merge logic respects Windows-style `Path` casing: if the merged env has a non-empty `Path` and no `PATH`, additions are written under `Path`; otherwise `PATH` is used. Existing entries are deduplicated before prepending so repeated executions don't grow the PATH unboundedly.

## Source

- `packages/adapters/pi-local/src/server/execute.ts` — implementation
- `server/src/__tests__/pi-local-execute.test.ts` — regression test that captures the spawned child's PATH
