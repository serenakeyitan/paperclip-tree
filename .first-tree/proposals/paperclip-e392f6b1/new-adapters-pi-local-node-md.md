---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The Pi adapter node states 'No quota reporting' under Boundaries, but this PR adds quota exhaustion handling that treats exhaustion as a failed run — the node needs updating.
---
Update the Boundaries section of the existing node. Replace 'No quota reporting.' with:

- **Quota exhaustion → failed run.** When Pi reports quota exhaustion, the adapter treats it as a terminal failure (failed run) rather than silently retrying or leaving the task in progress. This ensures the task system surfaces the failure for manual recovery, consistent with the 'no automatic recovery' principle.
- Model config is read-only. The adapter discovers but does not write Pi configuration.
