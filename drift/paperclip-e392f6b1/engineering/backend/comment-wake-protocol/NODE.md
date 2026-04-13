---
title: "Comment Wake Protocol"
owners: []
---

# Comment Wake Protocol

How Paperclip wakes agents in response to comments on their assigned issues, and the efficiency optimizations that prevent redundant wakes.

**Source:** `server/src/services/` (comment-related wakeup logic), `server/src/routes/issues/` (comment endpoints).

## Architecture

### Comment-Triggered Wakes

When a comment is posted on an issue with an assigned agent, Paperclip can trigger an agent wake so the agent reads and responds to the comment. This is distinct from scheduled heartbeat wakes — comment wakes are event-driven and immediate. The comment wake uses the same adapter execution path as heartbeat runs but with comment-specific context delivery (the new comment and its thread context).

### Wake Efficiency

Naive comment wakes are expensive: every comment would spawn an adapter execution. The comment wake protocol includes efficiency measures to avoid redundant wakes:
- **Debouncing** — rapid successive comments (e.g., during a human typing a multi-part message) are batched into a single wake.
- **Deduplication** — if the agent is already running (from a heartbeat or prior comment wake), a new comment wake is suppressed or queued.
- **Selective wake** — not every comment warrants a wake. System-generated comments, status change notifications, and comments from the agent itself do not trigger wakes.

## Key Decisions

- **Comment wakes are a separate wake trigger, not a heartbeat override.** Comment wakes coexist with scheduled heartbeats. They use the same execution infrastructure but have independent triggering logic.
- **Efficiency is server-side.** The debouncing and deduplication logic lives in the backend services, not in adapters. Adapters receive a wake request and execute; they don't know whether it was triggered by a comment, heartbeat, or dependency resolution.

## Boundaries

- The **heartbeat protocol** (scheduled wakes, context delivery spectrum) is defined in `product/agent-model`. This node covers **comment-triggered wakes** specifically.
- The **task system** owns the comment model (tasks are the communication channel). This node covers the **wake mechanism** that reacts to comments.
- **Execution workspace** setup during a comment wake follows the same worktree isolation pattern documented in the execution workspaces node.
