---
title: "Examples Live In-Tree As SDK Contract Tests"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Examples Live In-Tree As SDK Contract Tests

Paperclip's example plugins are not just marketing samples. They are part of
how the project proves the SDK stays usable.

## Decision

Keep example plugins inside the main workspace and treat them as both living
documentation and lightweight contract tests for the plugin SDK.

## Why

When examples compile against the current workspace version, they provide
copyable reference projects for authors while also exposing SDK breakage much
earlier than a separate documentation repo would.

## Implications

- SDK changes should consider whether one or more examples need updating as
  part of the same change.
- Example coverage should expand when a major plugin surface is added so the
  public contract has a concrete demonstration.
- Examples are intentionally reference implementations, not production plugins
  that define the runtime contract themselves.
- Test and documentation value are both reasons to keep these examples close to
  the SDK source.

## Related Domains

- [plugins sdk](../sdk/NODE.md)
- [infrastructure testing](../../infrastructure/testing/NODE.md)
