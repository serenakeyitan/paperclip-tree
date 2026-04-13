---
title: ""
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

Update the existing Claude Local Adapter node's **Session resume** bullet or **Key Decisions** section to add:

### System Prompt Skipped on Resume

`--append-system-prompt-file` is only passed on **new** sessions. When resuming via `--session-id`, the flag is omitted because the system prompt was already injected in the original session. Re-appending it would duplicate instructions and waste context window tokens.

**Rationale:** The system prompt file contains Paperclip's agent instructions and skill definitions. Claude Code persists these across the session, so re-injecting them on resume is redundant and can cause confusing duplicate behavior.
