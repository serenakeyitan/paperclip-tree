---
title: "Server Test Isolation"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Server Test Isolation

Server endpoint tests isolate route modules to prevent cross-test contamination and ensure each test exercises its route handler in a clean context.

## Convention

Each endpoint test file isolates its route module rather than importing from a shared server instance. This prevents module-level side effects (middleware registration, shared state) from leaking between test suites, which was causing flaky and order-dependent test failures.

## Why This Matters

Without route isolation, Node's module cache means that importing a route in one test can affect another test's behavior — middleware gets registered multiple times, shared state persists across suites, and tests pass individually but fail when run together. Isolating route modules makes the test suite deterministic regardless of execution order.

## Key Decision

- **Isolate route modules per test file.** Do not share a single server/app instance across endpoint test files. Each test should set up its own route context to avoid cross-contamination from Node module caching and Express middleware registration.
