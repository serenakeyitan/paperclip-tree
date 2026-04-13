---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The testing node documents test layers and frameworks but has no mention of server endpoint test isolation patterns — a convention this PR establishes for how route modules are isolated in unit tests.
---
### Server Endpoint Test Isolation

Server route endpoint tests isolate each route module to prevent cross-test contamination and ensure tests are independent. Rather than testing against a fully wired-up Express app, each test file imports and registers only the route module under test.

This pattern ensures:
- Tests don't depend on registration order or side effects from other route modules
- Each test suite has a predictable, minimal surface area
- Failures are attributable to the specific route module, not to interactions between modules

This convention was established in PR #3206 (`pap-1239-server-test-isolation`). All new server endpoint tests should follow this isolation pattern.
