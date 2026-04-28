---
title: "Built-in Routine Variable: timestamp"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [product/routines/NODE.md]
---

# Built-in Routine Variable: `timestamp`

Routine templates have a built-in variable namespace that is auto-populated at interpolation time without the routine author having to declare it. The set is intentionally small. It now contains two entries:

- `date` — current UTC date in `YYYY-MM-DD` form (pre-existing).
- `timestamp` — current UTC date and time in a human-readable form (e.g. `April 28, 2026 at 12:17 PM UTC`).

## Key Decisions

### Human-Readable, Not ISO

`timestamp` is intentionally formatted via `Intl.DateTimeFormat` in `en-US` with `hour12: true` and `timeZone: "UTC"` rather than emitting an ISO 8601 string. Routine prompts are consumed by LLM agents, and a human-readable phrasing reads more naturally inside instructions than `2026-04-28T12:17:00Z`. The format always includes year, time, and the literal string `UTC` so downstream parsers and humans can disambiguate without locale assumptions.

### Excluded From User Variable Sync

Like `date`, `timestamp` is excluded from `syncRoutineVariablesWithTemplate` — referencing `{{timestamp}}` in a template does not create a user-defined variable row. Built-in names are reserved; if an author defines a variable with the same name it does not shadow the built-in.
