---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The no-hardcoded-secret-fallbacks convention (BETTER_AUTH_SECRET must fail loudly) is a security decision not captured in any existing node.
---
# Security

Security conventions and constraints for the Paperclip platform.

## Key Decisions

### No Hardcoded Secret Fallbacks

Auth initialization must fail loudly at startup if `BETTER_AUTH_SECRET` is missing — never fall back to a hardcoded value. This is defense-in-depth independent of Docker Compose enforcement. A missing secret is a deployment error that must be caught immediately, not papered over with a default that could be exploited.
