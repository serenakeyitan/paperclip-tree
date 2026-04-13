---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Testing node documents test layers and frameworks but has no mention of server endpoint test isolation — a convention this PR establishes for how route modules are isolated in unit tests to prevent cross-test contamination.
---
## Server Endpoint Test Isolation

Server route endpoint tests isolate each route module to prevent cross-test contamination and ensure tests are independent. Rather than testing against a fully wired-up Express app, each test file imports and registers only the route module under test.

This pattern ensures:
- Tests don't depend on registration order or side effects from other route modules
- Each test suite has a predictable, minimal surface area
- Failures are attributable to the specific route module, not to interactions between modules

Established in PR #3206 (`pap-1239-server-test-isolation`). All new server endpoint tests should follow this isolation pattern.
