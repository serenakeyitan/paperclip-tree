---
title: "Dialog Context: State vs Actions Hooks"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/NODE.md]
---

`DialogContext` exposes two hooks: `useDialog` for components that need dialog state, and `useDialogActions` for components that only need to invoke open/close actions (e.g., `openNewIssue`, `openOnboarding`, `openNewAgent`).

## Why Two Hooks

Most callers of dialog APIs only need stable action callbacks, not the dialog state itself. Subscribing to the full context caused unnecessary re-renders when dialog state changed. `useDialogActions` returns a referentially stable actions object so consumers like `Layout`, `CompanyRail`, `CommandPalette`, and `IssuesList` are not re-rendered when an unrelated dialog opens or closes.

## How to Apply

Prefer `useDialogActions` whenever a component only triggers dialogs. Reserve `useDialog` for components that render based on dialog state (the dialog host itself, anything reading `isOpen`-style fields). Test mocks for `../context/DialogContext` should provide both hooks so consumers under test continue to work after migration.
