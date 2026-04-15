---
title: "Static Asset Serving & SPA Fallback"
owners: [bingran-you, cryppadotta, serenakeyitan, stubbi]
---

# Static Asset Serving & SPA Fallback

How the Express server serves the built frontend UI when `SERVE_UI=true`.

## Static Asset Cache Headers

The server sets explicit `Cache-Control` headers on static assets (JS, CSS, images, fonts) served from the built UI directory. Hashed assets get long-lived cache headers; non-hashed assets (like `index.html`) get short or no-cache headers to ensure users always get the latest version.

## SPA Fallback

For single-page app routing, the server returns `index.html` for any non-API route that doesn't match a static file. This allows React Router (client-side routing) to handle navigation without 404s. API routes (`/api/*`, `/auth/*`, WebSocket endpoints) are excluded from the fallback and handled by their respective Express route handlers.

## Key Decisions

### Cache-busted assets get aggressive caching

Vite-built assets include content hashes in their filenames, so they can be cached indefinitely. The server leverages this by setting long `max-age` on hashed assets while keeping `index.html` fresh. This is a standard SPA caching pattern that works correctly with CDNs and browser caches.

### Fallback ordering matters

The SPA fallback must be registered after all API routes and static file middleware. If registered too early, it would intercept API requests and return HTML instead of JSON. The ordering in `app.ts` is: API routes → static file middleware → SPA fallback.
