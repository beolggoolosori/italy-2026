# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page travel-planning dashboard for a 2026 Italy couple trip (2026-09-16 to
2026-09-28, 12 nights / 13 days). Pure static HTML with no build step, no package
manager, and no backend. Leaflet (maps) is the only runtime dependency, loaded from a
CDN. The site is published via GitHub Pages, served directly from `index.html`.

All user-facing content is in Korean. English is used only for URLs, official place/hotel
names, and code identifiers.

## Run & verify locally

```bash
python3 -m http.server 8080
# then open http://localhost:8080/index.html
```

There are no tests, linters, or build commands — verification is manual in the browser.
After editing the dashboard, check:
- page loads with no JavaScript console errors,
- the Leaflet map renders,
- course tabs and section tabs switch correctly,
- mobile width does not overlap text,
- the map/route data matches the selected course.

## File layout & versioning

- `index.html` — the current published dashboard. **Byte-identical to `v4.html`.**
- `v1.html` … `v4.html` — historical top-level snapshots; `v4` is current.
- `plan/VERSIONS.md` — the version map / changelog (Korean). Update it when you cut a new
  dashboard version.
- `AGENTS.md` — detailed project rules and trip facts (read it; richer than this file).
- `.gitignore` is allowlist-style: **only `*.html`, `AGENTS.md`, and `plan/VERSIONS.md`
  are tracked.** Everything else (research notes, spreadsheets, agent prompts referenced
  in `AGENTS.md`) is intentionally untracked and absent from a fresh clone.

When editing the live site, edit `index.html` (and keep `v4.html` in sync). Treat `v1`–`v3`
as read-only history. Only publish a new version into `index.html` when the user asks.

## Architecture (all inside index.html)

Each `*.html` is fully self-contained: CSS in `<style>`, data + logic in one `<script>`.
There is no shared/external JS or CSS. The dashboard is data-driven from a handful of JS
objects defined near the top of the script:

- `COURSES` — the core data model, keyed by course id (`A`, `B`, `C`). Each course holds
  `budget`, `cities`, `calendar` (day-of-month → cell), `transport`, `hotels`
  (city → candidates), and `days` (day-by-day `sched` steps). Default selected course is
  `currentCourse = 'B'`.
- `FOOD_RECS`, `FOOD_GUIDE` — restaurant recommendations and food guide content.
- `COMPARE`, `BOOKING_ITEMS`, `TICKETING_ITEMS` — trip summary and booking/ticketing
  checklist data (with KST-based booking calendar).
- `SEC_LIST` — defines the section tabs (지도/달력/일정/음식/이동수단/예약/숙소/예산/요약).

Rendering: `init()` builds the course tabs and section tabs, then per-section `render*`
functions (`renderItinerary`, `renderCalendar`, `renderTransport`, `renderBooking`,
`renderHotels`, `renderFoodGuide`, `renderStats`, etc.) regenerate DOM from `COURSES`.
`switchCourse(id)` re-renders everything for the chosen course. Two Leaflet maps are
managed: the overview map (`updateMap`) and the per-day itinerary map (`initDayMap` /
`showDayOnMap`). Marker placement has custom geometry helpers (`markerLabelOffsets`,
`createNumberedIcon`, route anchor/arrow icons, haversine `distanceKm`).

A `sched` step object drives both the itinerary table and the map. Notable fields:
`coords` (lat/lng), `type` (`food`/`spot`/`move`/`hl`/`couple`/`option` → marker color via
`ICON_COLORS`), `map:false` (hide from map), `route:false` / `optional` (exclude from the
drawn route line), and `actions` (booking links). When adding itinerary steps, set these
consistently or markers/route lines will desync from the table.

## Data integrity rules (from AGENTS.md)

- Keep route data, calendar, transport rows, budget rows, and visible tab labels
  consistent within a course — they are read from the same `COURSES[id]` and must agree.
- Dates, prices, opening hours, ticket windows, and booking links are time-sensitive.
  Verify against official/primary sources before changing them, and mark assumptions with
  a verification date (the booking section already shows a "검증 기준일").
- Do not overwrite existing research; add revisions or a new versioned artifact.
- Respond to the user in Korean unless asked otherwise.
