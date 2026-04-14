---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Testing node documents test layers and frameworks but says nothing about server route test isolation patterns — this PR introduces module isolation for endpoint tests, which is a key testing convention.
---
### Server Route Test Isolation

Endpoint tests for the backend server isolate route modules to prevent cross-test contamination. Each test file loads its route module in isolation rather than importing the full Express app, ensuring tests are independent and deterministic. This was introduced in PR #3206 to fix flaky or coupled test behavior in the server route test suite.
