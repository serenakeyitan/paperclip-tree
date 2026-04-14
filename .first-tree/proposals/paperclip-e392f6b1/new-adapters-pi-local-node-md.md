---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR title 'Treat Pi quota exhaustion as a failed run' adds quota-exhaustion-as-failure semantics to the Pi adapter, and the existing node explicitly states 'No quota reporting' under Boundaries — this needs updating.
---
Under **Key Decisions**, add:

### Quota Exhaustion as Run Failure

When the Pi adapter detects quota exhaustion (rate limits or usage caps from the Pi provider), it treats this as a failed run rather than a retriable transient error. This ensures the heartbeat orchestration layer does not endlessly retry runs that will consistently fail due to quota, and surfaces the problem to the board for intervention.

Under **Boundaries**, update:
- ~~No quota reporting.~~ → Quota exhaustion is detected and surfaced as a run failure, but detailed quota metrics (remaining balance, reset time) are not reported.
