---
title: "Execution Policies"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Execution Policies

Configurable sign-off-required policies that designate action categories requiring human approval before execution. The Board defines which actions need sign-off; enforcement happens at the server layer using the same blocking model as existing approval gates.

## Key Decisions

### Server-Side Enforcement

Execution policies are enforced at the server layer, not the adapter or UI layer. This ensures policies cannot be bypassed regardless of how an action is initiated (UI, API, MCP, or CLI).

### Board-Designated Categories

The Board (governance authority) designates which action categories require approval. This keeps governance decisions centralized with the appropriate authority rather than distributed across individual agent configurations.
