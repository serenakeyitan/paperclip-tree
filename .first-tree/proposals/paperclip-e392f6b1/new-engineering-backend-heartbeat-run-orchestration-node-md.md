---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR handles empty dev runner responses (1de1393) as a failure-hardening measure, and the Pi quota exhaustion as failed run also affects run orchestration failure handling.
---
Under **Failure Hardening**, consider adding:

### Empty Dev Runner Response Handling

The orchestration layer handles empty responses from the dev runner gracefully rather than treating them as successful completions or crashing. This prevents silent failures when the dev runner process exits without producing output.

### Adapter-Specific Failure Classification

Adapter-specific error conditions (e.g., Pi quota exhaustion) are classified as run failures at the orchestration level, ensuring the system does not retry runs that will consistently fail due to external provider constraints.
