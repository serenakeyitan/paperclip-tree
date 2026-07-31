---
title: "Access Service"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

The access service is the backend component that enforces multi-user access control: who belongs to a deployment, what role they hold, and which join requests are pending. It is the server-side counterpart to the invite-flow product spec and the single point every authenticated route should consult before performing a mutation.

The service is backed by the `join_requests` table (`packages/db/src/schema/join_requests.ts`, migration `0057_tidy_join_requests.sql`) and consumes the access types and validators from the shared package (`packages/shared/src/types/access.ts`, `packages/shared/src/validators/access.ts`). Behavior is covered by `server/src/__tests__/access-service.test.ts` and `access-validators.test.ts`; adapter-route tests (`adapter-routes.test.ts`) were updated to reflect the new access checks on adapter endpoints.

The service is shared across server routes rather than duplicated per-feature: new endpoints that touch user-owned state should call into it instead of re-implementing role checks. The first-invite bootstrap path (`packages/db/scripts/create-auth-bootstrap-invite.ts`) is the only sanctioned way to seed the initial admin, so production provisioning scripts and docs (`doc/DEPLOYMENT-MODES.md`, `doc/PUBLISHING.md`) route through it.

Company-scoped isolation still holds: access roles live alongside, not instead of, `company_id` scoping. A user with a role in one deployment has no implicit access to another.
