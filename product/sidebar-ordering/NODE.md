Paperclip persists per-user sidebar ordering so that each operator sees companies and projects in the order they have arranged them, independent of other users.

## Two Scopes

- **Company order** — global per user. Stored in `user_sidebar_preferences` keyed uniquely by `user_id`, with `company_order` as a JSONB array of company ids.
- **Project order** — per (company, user). Stored in `company_user_sidebar_preferences` keyed uniquely by `(company_id, user_id)`, with `project_order` as a JSONB array of project ids. Rows are cascade-deleted when the company is deleted.

Both records carry `created_at` / `updated_at` and are upserted when the user reorders the sidebar.

## API Surface

A new `sidebar-preferences` API namespace (`/api/sidebar-preferences`) exposes the read/upsert endpoints. The shared package defines `SidebarOrderPreference` (`{ orderedIds, updatedAt }`) and `upsertSidebarOrderPreferenceSchema` as the wire contract between server and UI.

## UI Behavior

`CompanyRail` and `SidebarProjects` read the stored order to render the sidebar and write the new order back on drag/reorder. Unknown ids (newly created or not yet in the stored order) fall back to the backend's default ordering so preferences degrade gracefully when membership changes.
