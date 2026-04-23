---
title: "User Profile Page"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# User Profile Page

Each company member has a profile page at `/:companySlug/u/:userSlug` (with an unprefixed redirect at `/u/:userSlug`). The page summarizes a user's participation in the company: identity, activity, cost, and issue involvement.

## Data Shape

The profile is served by `GET /companies/:companyId/users/:userSlug/profile` and returns a `UserProfileResponse` composed of:

- **Identity** — slug, name, email, image, membership role + status, join date.
- **Window stats** — three fixed windows (`last7`, `last30`, `all`) each reporting touched/created/completed/assigned-open issues, comment and activity counts, cost cents, and token totals (input, cached input, output) plus cost-event count.
- **Daily points** — per-day activity count, completed issues, cost cents, and token breakdown for trend charts.
- **Agent and provider usage** — cost/usage rollups per agent and per provider.
- **Issue summary** — recent issues the user created, was assigned to, or acted on.

## Key Decisions

### Fixed Windows, Not Arbitrary Ranges

Window keys are a closed set (`last7 | last30 | all`). This keeps server aggregation predictable and cacheable; ad-hoc ranges are intentionally out of scope.

### Slug-Addressable Users

The route keys on `userSlug`, not user UUID, so profile URLs are shareable and stable across renames of the underlying auth record.

### Cost Attribution Lives Here

User-level cost and token aggregation (by window, by day, by agent, by provider) is materialized on this endpoint rather than in a generic cost API, because the profile is the primary consumer and needs them pre-joined with activity counts.
