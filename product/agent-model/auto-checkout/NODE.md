---
title: "Auto-Checkout on Issue Wake"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Auto-Checkout on Issue Wake

When an agent wakes for a scoped issue, the execution harness automatically checks out the git branch associated with that issue before the agent receives its task prompt.

## Key Decisions

### Harness-Level, Not Agent-Level

Auto-checkout runs in the harness before the agent starts, not as an agent action. This ensures the agent always begins in the correct workspace context without needing to understand checkout mechanics.

### Scoped Issues Only

Auto-checkout activates only for issue-scoped wakes. General agent wakes (health checks, idle polls) do not trigger checkout. The checkout target follows the issue's branch, not the agent's last-known branch state.
