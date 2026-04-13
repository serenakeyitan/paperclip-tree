---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR fixes onboarding to use relative paths instead of $AGENT_HOME in CEO instructions — this portability convention for generated agent instructions is not documented in the CLI node or anywhere in the tree.
---
# Onboarding & CEO Bootstrap

The interactive setup flow that initializes a new Paperclip instance and bootstraps the CEO agent.

## Key Decisions

### Relative Paths in CEO Instructions

CEO instructions use relative paths instead of absolute paths or environment variables like `$AGENT_HOME`. This ensures instructions are portable across environments — the same company definition works whether the agent runs locally, in Docker, or in a CI workspace. Absolute paths and env-var references break when the agent's home directory differs from the onboarding machine.

### CEO as First Agent

Onboarding creates the CEO agent as the first step after company creation. The CEO receives generated instructions that define its operating context, workspace layout, and initial responsibilities. These instructions are the seed from which the entire company structure grows.
