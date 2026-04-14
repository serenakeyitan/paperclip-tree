---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The testing node covers test layers and frameworks but says nothing about test isolation patterns for server route/endpoint tests, which this PR introduces.
---
### Server Route Test Isolation

Endpoint tests for backend routes isolate each route module so tests do not leak state or dependencies between suites. Route modules are loaded in isolation per test file, preventing shared mutable state (middleware, app-level side effects) from causing flaky or order-dependent test results. This pattern complements the route/service split architecture — since routes are thin HTTP handlers, isolating them in tests is straightforward and keeps the unit test layer fast and deterministic.
