---
title: "Frontend UI"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend", "engineering/shared", "adapters"]
---

# Frontend UI

React single-page application serving the Paperclip board interface. Source: `/ui`.

Package: `@paperclipai/ui`

## Stack

- **React 19** with TypeScript
- **Vite** for dev server and production builds
- **Tailwind CSS v4** for styling
- **Radix UI** primitives + **shadcn/ui** component patterns (via `components.json`)
- **TanStack React Query** for server state management
- **React Router v7** for client-side routing
- **Lexical** + **MDX Editor** for rich text editing (issue descriptions, documents)
- **Mermaid** for diagram rendering
- **@assistant-ui/react** for AI chat interface components

## Page Structure

Pages live in `/ui/src/pages/` and map to top-level routes:

- **Dashboard** — overview metrics and activity feed
- **Companies** — list and manage AI companies
- **Agents** — agent roster, configuration, status (with NewAgent creation flow)
- **Issues** — task board with detail view, chat UX, and inbox
- **Projects** — project grouping with workspace detail views
- **Goals** — OKR-style goal hierarchy
- **Routines** — scheduled automation management
- **Approvals** — human-in-the-loop review queue
- **Costs** — budget and spend tracking
- **Activity** — audit log and event timeline
- **Settings** — instance-level and company-level configuration
- **Plugins / Adapters** — extension and adapter management UIs
- **Auth / CliAuth / BoardClaim / InviteLanding** — authentication flows

## Architecture Patterns

- **API layer** (`/ui/src/api/`) centralizes all HTTP calls to the backend. Components never call `fetch` directly.
- **Context providers** (`/ui/src/context/`) manage cross-cutting state (current company, auth session, theme).
- **Hooks** (`/ui/src/hooks/`) encapsulate React Query patterns and shared UI logic.
- **Components** (`/ui/src/components/`) are reusable UI building blocks, following shadcn/ui conventions.
- **Adapter UI** (`/ui/src/adapters/`) — frontend components for adapter-specific configuration and display.

## Key Decisions

- **No global state library.** React Query handles server state; React context handles UI state. No Redux/Zustand.
- **UI is pre-built and published as an npm package** (`@paperclipai/ui`). The server can serve it as static assets or proxy to a Vite dev server during development.
- **Tailwind v4** (not v3) — uses the new CSS-first configuration approach.
- **Adapter UI components are co-located with adapter packages** but imported into the main UI bundle for the management pages.

## Sub-domains

- [inbox-list/](inbox-list/) — Inbox list rendering, workspace grouping, and collapse-state persistence
- [issue-document-freshness/](issue-document-freshness/) — Issue document editor freshness and save conflict handling
- [issue-list-ux/](issue-list-ux/) — Issue list/board rendering, filtering, and grouping UX
- [issue-thread-ux/](issue-thread-ux/) — Issue detail chat thread and comment UX
- [production-build-config/](production-build-config/) — Production Vite build transforms (drop `console.*`, strip legal comments)

## Decision Records

- [api-layer-and-react-query-over-global-state.md](api-layer-and-react-query-over-global-state.md) — Why frontend data flow is organized around a shared API layer plus React Query instead of a global store.
