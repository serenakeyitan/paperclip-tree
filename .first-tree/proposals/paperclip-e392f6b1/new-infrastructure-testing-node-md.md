---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The testing node documents test layers and frameworks but says nothing about server route test isolation patterns; this PR introduces a module isolation strategy for endpoint tests that future contributors need to know about.
---
### Server Route Test Isolation

Server endpoint tests isolate route modules to prevent cross-test pollution. Each test file loads its route module in isolation rather than importing the full Express app, ensuring tests are independent and don't leak state between runs. This pattern applies to the Vitest unit/integration tests under the server package.
