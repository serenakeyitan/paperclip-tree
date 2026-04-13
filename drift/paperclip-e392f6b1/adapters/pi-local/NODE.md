---
title: "Pi Local Adapter"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

Update the existing Pi Local Adapter node to reflect the new quota exhaustion behavior.

## Change Needed

Remove "No quota reporting" from the Boundaries section. Add to the error handling or run lifecycle section:

### Quota Exhaustion

When the Pi runtime reports quota exhaustion, the adapter treats it as a **failed run** rather than silently retrying or hanging. This ensures the orchestrator can reassign work or escalate, matching the fail-fast convention used by other adapters for unrecoverable provider errors.
