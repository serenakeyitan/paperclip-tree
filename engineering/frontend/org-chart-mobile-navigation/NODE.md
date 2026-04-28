---
title: "Org Chart Mobile Navigation"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/NODE.md]
---

# Org Chart Mobile Navigation

The OrgChart page (`ui/src/pages/OrgChart.tsx`) supports touch-based navigation so the agent hierarchy is usable on mobile and tablet, in addition to desktop mouse interactions.

## Gestures

A single `TouchGesture` state machine handles two modes:

- **Pan** — single-finger drag translates the chart. A `TOUCH_MOVE_THRESHOLD` of 6px distinguishes intentional drags from taps, so tapping an agent card still navigates instead of being swallowed by the gesture handler.
- **Pinch** — two-finger gesture zooms around the midpoint of the two touches, preserving the focal point under the user's fingers rather than zooming from the chart origin.

## Zoom Controls and Bounds

Explicit zoom in/out/reset buttons (`Plus`, `Minus`, `Maximize2` icons from lucide-react) are rendered alongside the chart for users without pinch-capable input. Zoom is clamped to `MIN_ZOOM = 0.2` and `MAX_ZOOM = 2` in both gesture and button paths so users cannot zoom into an unrecoverable state.

## Testing

`ui/src/pages/OrgChart.test.tsx` exercises the navigation surface with a jsdom environment, mocking the router, company context, and agents API. Tests cover tap-to-navigate (movement under threshold), pan suppression of navigation (movement over threshold), and the zoom control bounds.
