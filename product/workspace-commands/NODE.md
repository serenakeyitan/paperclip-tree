Paperclip models the runnable commands attached to a project or execution workspace as **workspace commands**, replacing the previous "raw runtime JSON" as the primary mental model. Raw runtime config is still accepted for advanced use, but operators and the UI think in terms of two kinds of commands.

## Two Kinds

- **Services** — long-running commands that stay supervised. They have a desired state (`running` or `stopped`) and are started/stopped from the workspace UI.
- **Jobs** — one-shot commands that run once and exit. They are triggered on demand from the workspace UI.

A `WorkspaceCommandDefinition` (`id`, `name`, `kind`, `command`, `cwd`, `lifecycle`, `serviceIndex`, `disabledReason`, `rawConfig`, `source`) is the canonical description shared between backend, shared package, and UI. Helpers `listWorkspaceCommandDefinitions`, `listWorkspaceServiceCommandDefinitions`, `findWorkspaceCommandDefinition`, and `matchWorkspaceRuntimeServiceToCommand` convert the stored runtime config blob into this model and reconcile it with live runtime service rows.

## Per-Service Desired State

Both `ProjectWorkspaceRuntimeConfig` and `ExecutionWorkspaceConfig` now carry an optional `serviceStates: WorkspaceRuntimeServiceStateMap` (`Record<string, 'running' | 'stopped'>`) alongside the legacy workspace-wide `desiredState`. This lets operators independently control each service within a workspace rather than flipping the whole workspace on or off.

## Control Target

Runtime control endpoints accept a `WorkspaceRuntimeControlTarget` (`workspaceCommandId` | `runtimeServiceId` | `serviceIndex`), so the UI can address a command by its stable definition id, by the live runtime service row, or by positional index — whichever is available at the call site.

## Inheritance

Execution workspaces still inherit the project workspace's command definitions by default and may override them. Paperclip does not auto-start or auto-stop workspace services as part of issue execution or server boot — all lifecycle transitions are explicit UI actions.
