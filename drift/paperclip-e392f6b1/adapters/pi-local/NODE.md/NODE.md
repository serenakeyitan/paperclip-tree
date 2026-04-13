---
title: "Pi Local Adapter"
owners: []
---

Update the Boundaries section of the existing node. Replace 'No quota reporting.' with:

- **Quota exhaustion → failed run.** When Pi reports quota exhaustion, the adapter treats it as a terminal failure (failed run) rather than silently retrying or leaving the task in progress. This ensures the task system surfaces the failure for manual recovery, consistent with the 'no automatic recovery' principle.
- Model config is read-only. The adapter discovers but does not write Pi configuration.
