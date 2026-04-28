---
title: "Workspace Env Propagation"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [adapters/NODE.md, adapters/claude-local/NODE.md, adapters/codex-local/NODE.md, adapters/cursor-local/NODE.md, adapters/gemini-local/NODE.md, adapters/opencode-local/NODE.md, adapters/pi-local/NODE.md, engineering/execution-workspaces/NODE.md, product/agent-model/NODE.md]
---

# Workspace Env Propagation

How Paperclip propagates workspace identity and agent home directory into the environment of spawned adapter processes.

## Shared Helper

All adapters (claude-local, codex-local, cursor-local, gemini-local, opencode-local, pi-local) use a single `applyPaperclipWorkspaceEnv` helper from `@paperclip/adapter-utils` to inject workspace env vars into the child process environment. This replaces ad-hoc per-adapter `if (x) env.X = x` blocks that previously drifted between adapters and made it easy to forget a variable in one place.

The helper writes the following keys when the corresponding input value is a non-empty string: `PAPERCLIP_WORKSPACE_CWD`, `PAPERCLIP_WORKSPACE_SOURCE`, `PAPERCLIP_WORKSPACE_STRATEGY`, `PAPERCLIP_WORKSPACE_ID`, `PAPERCLIP_WORKSPACE_REPO_URL`, `PAPERCLIP_WORKSPACE_REPO_REF`, `PAPERCLIP_WORKSPACE_BRANCH`, `PAPERCLIP_WORKSPACE_WORKTREE_PATH`, and `AGENT_HOME`. Empty strings, null, and undefined are skipped, so callers can pass partial workspace info without polluting the env with empty keys.

## AGENT_HOME Contract

`AGENT_HOME` is the agent's persistent home directory inside the workspace and is the canonical anchor for agent-local state (memory, plans, life/PARA notes). Agent-facing onboarding assets — including the CEO `HEARTBEAT.md` — must reference `$AGENT_HOME/memory/...` and `$AGENT_HOME/life/...` rather than relative `./memory/` or `./life/` paths, because the working directory the agent runs in is not guaranteed to be the agent home (especially under git-worktree workspace strategies).

## Adding New Adapters or Variables

New adapters should call `applyPaperclipWorkspaceEnv` rather than re-implementing the per-key conditionals. New workspace-scoped env vars should be added to the helper's mappings (and its test) so every adapter picks them up automatically.
