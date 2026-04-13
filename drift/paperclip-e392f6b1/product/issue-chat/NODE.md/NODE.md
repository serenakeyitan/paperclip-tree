---
title: "Issue Chat UX"
owners: []
---

## Overview

Issue comments and agent run transcripts are rendered as a chat-like thread using the assistant-ui library. This replaces the traditional comment list with a conversational UX that makes agent interactions feel like messaging.

## Key Decisions

- **assistant-ui integration** — The chat thread is built on assistant-ui, which provides the message rendering primitives. Agent chain-of-thought output is mapped into assistant-ui message types.
- **Run transcript presentation** — Agent runs appear inline in the chat thread with shimmer animations for in-progress work (Working.../Worked tokens). Interrupted and non-succeeded runs are always rendered with their transcript.
- **Reusable run feed** — The chat-style run feed component is shared between the issue detail page and the dashboard, ensuring consistent presentation.
- **Crash guarding** — The issue chat is wrapped with error boundaries to guard against assistant-ui crashes, preventing a single rendering failure from breaking the entire issue detail view.
- **Inline composer** — The comment composer is inline within the chat thread with a capped height, stays visible while typing, and supports keyboard shortcuts (g c to focus).
