---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The testing node describes test layers and frameworks but does not capture the convention of isolating route modules in server endpoint tests — a structural testing pattern that affects how all future server tests should be written.
---
# Server Test Isolation

Server endpoint tests isolate route modules to prevent cross-test contamination and ensure each test exercises its route handler in a clean context.

## Convention

Each endpoint test file isolates its route module rather than importing from a shared server instance. This prevents module-level side effects (middleware registration, shared state) from leaking between test suites, which was causing flaky and order-dependent test failures.

## Why This Matters

Without route isolation, Node's module cache means that importing a route in one test can affect another test's behavior — middleware gets registered multiple times, shared state persists across suites, and tests pass individually but fail when run together. Isolating route modules makes the test suite deterministic regardless of execution order.

## Key Decision

- **Isolate route modules per test file.** Do not share a single server/app instance across endpoint test files. Each test should set up its own route context to avoid cross-contamination from Node module caching and Express middleware registration.
