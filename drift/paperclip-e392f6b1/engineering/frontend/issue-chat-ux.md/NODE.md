---
title: "Issue Chat UX"
owners: []
---

## Issue Chat UX

Issue detail pages now feature a chat-like conversation thread built on `@assistant-ui/react`, replacing the traditional linear comment view with an interactive chat experience.

### Key Features

- **Chat-style comments** — User and agent messages are rendered in a chat bubble layout with avatars, action bars, and relative date formatting.
- **Chain-of-thought rendering** — Agent reasoning steps are mapped and rendered inline within the chat thread, with collapsible work sections.
- **Run transcripts** — Agent execution runs are displayed as transcript messages within the chat flow. Interrupted and non-succeeded runs have explicit visual treatment.
- **Shimmer animation** — Active agent work shows a shimmer effect with "Working"/"Worked" status tokens. Animation uses oklch colors with pause-between-repeats to avoid jitter.
- **Image gallery** — Image attachments in chat messages open a gallery viewer on click.
- **Crash resilience** — The chat thread is wrapped in error boundaries to guard against assistant-ui crashes.

### Decisions

- Built on `@assistant-ui/react` (already a project dependency) rather than a custom chat implementation.
- Chat-style run feed is reused on the dashboard, not just issue detail.
- Composer stays inline and visible while typing, with a height cap to prevent layout shifts.
- Auto-folding of work sections on page load is handled without visual flash.
