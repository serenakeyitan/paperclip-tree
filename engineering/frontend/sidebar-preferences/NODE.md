---
title: "Sidebar Preferences"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Sidebar Preferences

User-scoped UI preferences for the sidebar, persisted server-side and synchronized across sessions. Covers the full vertical: database schema, API routes, shared validators, and frontend consumption.

**Source:** `packages/db/src/schema/user_sidebar_preferences.ts`, `packages/db/src/schema/company_user_sidebar_preferences.ts`, `server/src/routes/sidebar-preferences.ts`, `packages/shared/src/types/sidebar-preferences.ts`, `packages/shared/src/validators/sidebar-preferences.ts`

## Key Decisions

### Company-Scoped User Preferences

Sidebar preferences use a two-table schema: `user_sidebar_preferences` stores user-level defaults, while `company_user_sidebar_preferences` stores per-company overrides. This follows the existing company-scoped isolation pattern — a user who belongs to multiple companies can have different sidebar layouts in each.

### Dedicated API Surface

Sidebar preferences are served through a dedicated `sidebar-preferences` route module rather than being folded into the existing user or company routes. This keeps the preference CRUD isolated and avoids coupling sidebar concerns with unrelated user/company mutations.

### Shared Validators

Preference shapes are defined once in the shared package (`sidebar-preferences.ts` validators) and consumed by both the API routes and the frontend. This ensures the contract between client and server stays in sync without runtime type mismatches.

## Boundaries

- The sidebar preferences system manages *persistence and retrieval* of preference data. It does not control sidebar rendering or layout logic — that is a frontend concern.
- Plugin sidebar slots (defined in the Plugin SDK) are separate from user sidebar preferences.
