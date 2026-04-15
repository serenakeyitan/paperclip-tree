---
title: "Static Asset Serving & SPA Fallback"
owners: [bingran-you, cryppadotta, serenakeyitan, stubbi]
---

# Static Asset Serving & SPA Fallback

How the Express server serves the built frontend UI when `SERVE_UI=true`.

## Static Asset Cache Headers

The server sets explicit `Cache-Control` headers on static assets (JS, CSS, images, fonts) served from the built UI directory. Hashed assets get long-lived cache headers; non-hashed assets (like `index.html`) get short or no-cache headers to ensure users always get the latest version.

## SPA Fallback

The fallback invariant: non-asset routes can fall back to `index.html`, but missing `/assets/*` paths must return 404. This prevents the HTML shell from being cached as a JS module when the browser requests a hashed asset URL that no longer exists (e.g., after a deploy). API routes (`/api/*`, including `/api/auth/*`) and WebSocket endpoints are excluded from the fallback and handled by their respective Express route handlers.

## Key Decisions

### Cache-busted assets get aggressive caching

Vite-built assets include content hashes in their filenames, so they can be cached indefinitely. The server leverages this by setting long `max-age` on hashed assets while keeping `index.html` fresh. This is a standard SPA caching pattern that works correctly with CDNs and browser caches.

### Fallback ordering matters

The SPA fallback must be registered after all API routes and static file middleware. If registered too early, it would intercept API requests and return HTML instead of JSON. The ordering in `app.ts` is: API routes → static file middleware → SPA fallback.
