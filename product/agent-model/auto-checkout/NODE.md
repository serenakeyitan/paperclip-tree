---
title: "Auto-Checkout on Issue Wake"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Auto-Checkout on Issue Wake

When an agent receives a **scoped issue wake** — a signal that a specific issue needs attention — the execution harness automatically checks out the relevant branch or worktree before the agent begins work. This eliminates a manual step where agents had to determine and switch to the correct working context.

## Key Decisions

### Harness-level automation

Auto-checkout is handled by the execution harness, not by individual agents or adapters. This ensures consistent behavior across all agent types — whether Claude, Codex, or any other adapter, the workspace is ready before the agent's first prompt.

### Scoped to issue context

The auto-checkout only activates for issue-scoped wakes, not general agent wakes. This prevents unnecessary workspace churn when an agent is woken for non-issue work (e.g., system health checks, scheduled maintenance tasks).

### Checkout target follows the issue, not the agent

The checkout target is determined by the issue's associated branch, not the agent's last-known state. If an agent is reassigned to a different issue, it checks out the new issue's branch. This follows the task-system principle that issues own the work context, not agents.
