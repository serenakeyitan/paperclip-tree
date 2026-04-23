---
title: "Better Auth Cookie Scoping"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Better Auth Cookie Scoping

Better Auth uses a per-instance cookie prefix so multiple Paperclip servers running on the same host do not overwrite each other's session cookies.

**Source:** `server/src/auth/better-auth.ts`

## Key Decisions

### Instance-Scoped Cookie Prefixes

`deriveAuthCookiePrefix()` derives Better Auth's `advanced.cookiePrefix` from `PAPERCLIP_INSTANCE_ID`, producing cookies like `paperclip-<instance-id>`. This prevents multiple local servers bound to the same hostname from clobbering each other's sessions, which can happen because browser cookies are scoped by host rather than by port.

### Sanitized and Stable Prefix Generation

The derived prefix sanitizes the instance ID into a cookie-safe segment and falls back to `paperclip-default` when no usable instance ID is available. This keeps auth behavior deterministic even when the raw instance ID contains unsupported characters or resolves to an empty value.

### Cookie Security and Cookie Scoping Are Independent

The instance-scoped prefix is always applied, including local HTTP development flows where secure cookies are disabled. `useSecureCookies` controls transport security; `cookiePrefix` controls session isolation. Paperclip configures both so local development can remain usable without reintroducing cross-worktree session collisions.

## Related

- Worktree and local dev instance IDs are part of the dev environment tooling documented in `engineering/backend/dev-runner/worktree-dev-tooling/NODE.md`.

## Boundaries

- This node covers auth-session isolation behavior in the backend auth layer.
- General worktree provisioning and local dev tooling stay in `engineering/backend/dev-runner/`.
