---
title: "Workspace Runtime Stale Command Reconciliation"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/execution-workspaces/NODE.md, engineering/frontend/NODE.md, engineering/shared/NODE.md]
---

# Workspace Runtime Stale Command Reconciliation

When a workspace's runtime config command changes (e.g. `pnpm dev` → `pnpm dev:once --tailscale-auth`), previously-started runtime services may still exist with the old command string. The UI reconciles these against the current command definitions using rules in `packages/shared/src/workspace-commands.ts` and `WorkspaceRuntimeControls.tsx`.

## Key Decisions

### Command-String Mismatch Disqualifies Match

`scoreWorkspaceRuntimeServiceMatch` returns `-1` when both the command definition and the runtime service have a `command` string and they differ. This prevents a stale runtime service from being matched to a command whose configured command has since changed, so the UI shows the updated command rather than masking it with the old service.

### Hide Stopped Stale Services, Surface Running Ones

In `buildWorkspaceRuntimeControlSections`, unmatched runtime services are filtered to only those in `starting` or `running` state before being rendered as "other services." Stopped stale services are dropped entirely — the user sees the updated command in its normal slot. Running stale services are surfaced separately so the user can see (and stop) them, since they're still consuming resources under the old command.

## Why

Without these rules, editing a workspace command would either silently leave the old service running under the matched slot (hiding the change) or leave dead stopped entries cluttering the UI. The split — disqualify on mismatch, hide-if-stopped, surface-if-running — keeps the control panel honest about what's actually executing.
