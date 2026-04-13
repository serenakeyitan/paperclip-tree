---
title: "Agent Environment Bindings"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Agent Environment Bindings

Agents can have per-agent environment variable bindings configured through the Paperclip UI. These bindings are injected into the agent's execution environment when it runs, allowing operators to pass secrets, API keys, and runtime configuration without hardcoding them into adapter configs or workspace files.

## Key Decisions

### UI-Managed Env Bindings

Environment bindings are a first-class configuration surface on the agent settings page. Operators can add, edit, and clear environment variables per agent. Clearing a binding and saving must persist the removal — the UI treats empty/cleared bindings as an explicit deletion, not a no-op.

### Distinct from Workspace Environment

Agent env bindings are separate from the worktree/workspace environment isolation handled by the dev runner. Workspace env isolation prevents leaking the parent process environment into agent workspaces. Agent env bindings are the intentional, operator-configured variables that *should* be present in the agent's runtime.

### Adapter-Agnostic

Env bindings are managed at the Paperclip control plane level and injected regardless of adapter type. The adapter does not need to implement env binding support — Paperclip handles injection into the execution context before the adapter spawns the agent process.
