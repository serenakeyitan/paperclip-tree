---
title: "Server Test Isolation"
owners: []
---

### Server Endpoint Test Isolation

Server route endpoint tests isolate each route module to prevent cross-test contamination and ensure tests are independent. Rather than testing against a fully wired-up Express app, each test file imports and registers only the route module under test.

This pattern ensures:
- Tests don't depend on registration order or side effects from other route modules
- Each test suite has a predictable, minimal surface area
- Failures are attributable to the specific route module, not to interactions between modules

This convention was established in PR #3206 (`pap-1239-server-test-isolation`). All new server endpoint tests should follow this isolation pattern.
