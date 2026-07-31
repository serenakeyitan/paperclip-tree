---
title: "Multi-User Access & Invites"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

Paperclip instances now support multiple human users per deployment, with access granted through an invite-and-join-request flow rather than a single-operator model. This shifts the governance surface from "one owner" to "a set of users with roles," and establishes the primitives other governance features (approvals, budgets, audit) will bind to.

Access is modeled as typed roles defined in `packages/shared/src/types/access.ts` with matching validators in `packages/shared/src/validators/access.ts`. A new `join_requests` table (migration `0057_tidy_join_requests.sql`, schema in `packages/db/src/schema/join_requests.ts`) tracks pending requests to join a deployment. The server exposes these flows through an access service (`server/src/__tests__/access-service.test.ts`, `access-validators.test.ts` cover behavior).

Bootstrapping a fresh deployment uses `packages/db/scripts/create-auth-bootstrap-invite.ts` to mint the first invite, so the initial admin is always established through the same invite primitive rather than an implicit superuser. The end-to-end flow — who can invite, how requests are approved, and how roles compose with existing company-scoped isolation — is specified in `doc/spec/invite-flow.md`.

Deployment implications are captured in `doc/DEPLOYMENT-MODES.md` and `doc/PUBLISHING.md`: operators must now decide whether a deployment is single-user or multi-user, and the bootstrap invite script is part of the install path. Future governance decisions (approval gates, budget ownership, audit actor identity) should reference access roles defined here rather than introducing parallel user models.
