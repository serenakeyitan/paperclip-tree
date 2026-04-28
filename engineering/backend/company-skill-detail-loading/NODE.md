---
title: "Company Skill Detail Loading"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, engineering/backend/NODE.md, engineering/shared/NODE.md]
---

# Company Skill Detail Loading

Loading company skill detail in the backend (`server/src/services/company-skills.ts`) intentionally avoids probing agent runtime adapters. The `actualState` field on `CompanySkillUsageAgent` is returned as `null` for operator-facing skill detail reads, even though the type permits a runtime-fetched value.

## Key Decision

### Skip Runtime Adapter Probes on Detail Reads

When the operator UI fetches company skill detail, the service does not call `findActiveServerAdapter().listSkills()` per agent to populate `actualState`. Probing every agent runtime serially made detail loads slow and added failure modes (offline adapters, timeouts) to a read path that operators expect to be snappy.

Instead, `actualState` is left as `null` and only populated when a caller explicitly fetches runtime adapter skill state through a different path. The shared type and Zod validator both document this contract: `actualState` is nullable and described as "Runtime adapter skill state when explicitly fetched; company skill detail reads return null without probing agent runtimes."

### Implications

- UI consumers must treat `actualState: null` as "unknown / not probed," not as "adapter reports no state."
- Any feature that needs the live adapter state must call the explicit runtime-state path, not rely on the detail endpoint.
- Tests under `server/src/__tests__/company-skills-detail.test.ts` lock in this behavior by mocking `listSkills` as a never-resolving promise and asserting detail still returns promptly.
