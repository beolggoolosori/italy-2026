# AGENTS.md - Italy Trip 2026 Codex Setup

## Project Scope

This project plans a 2026 Italy couple trip. Existing work was created with Claude; Codex should preserve that work and continue from the current artifacts instead of restarting.

- Trip dates: 2026-09-16 (Wed) to 2026-09-28 (Mon), 12 nights / 13 days.
- Travelers: couple, 2 people.
- International route in project docs: ICN -> Rome FCO on KE931, Milan MXP -> ICN on KE928.
- Direction: south to north within Italy.
- Repository root: `/Users/bell/Travel/italy/20260916-20260928`.
- Git remote: `git@github.com-personal:beolggoolosori/italy-2026.git`.

## Source Of Truth

Use these files to understand the current state before editing:

- `CLAUDE.md`: original Claude team-agent overview.
- `agents/*.md`: role prompts from the Claude workflow.
- `plan/VERSIONS.md`: version map. It was stale before Codex setup, so cross-check against `index.html` and Git history.
- `index.html`: current public/shared static page; currently identical to `v4.html` and newer than `plan/italy-planner.html`.
- `v4.html`: snapshot matching `index.html`.
- `plan/italy-planner.html`: older planning dashboard retained as an archive/reference.
- `v1.html` to `v4.html`: historical top-level dashboard snapshots.
- `research/이탈리아.xlsx`: original spreadsheet draft with sheets `Sheet1`, `마테라제외`, and `남부제외`.
- `research/*.md`: detailed research by topic.

Important: root `index.html` / `v4.html` and `plan/italy-planner.html` are not identical. Use `index.html` / `v4.html` for the public/shared dashboard unless the user explicitly asks to edit the archived planner in `plan/`.

## Current Planner Options

The current public dashboard (`index.html` / `v4.html`) compares three routes:

- Course A: Naples / Amalfi, excludes Matera, includes 3 nights in Florence and a Pisa day trip.
- Course B: excludes southern Italy, focuses central/northern Italy, Cinque Terre, Lake Como, and rental car flexibility.
- Course C: Dolomites 3 nights, rental car, Seceda / Carezza / Braies focus, then Lake Como / Milan.

The archived `plan/italy-planner.html` compares four routes, and older Markdown plans contain five-course comparisons. Treat those as historical context unless the user asks to revive them.

## Agent Roles

Continue using the existing specialist-agent model:

- Coordinator: route strategy, day allocation, conflict checks, final integration.
- Flight / Transport: international flights, trains, buses, ferries, airport links, booking links.
- Accommodation: city-by-city lodging areas, hotel options, price bands, cancellation policies.
- Itinerary: day-by-day flow, attraction order, opening-day conflicts, walking load.
- Food: restaurants, local dishes, reservations, meals mapped to route timing.
- Budget: EUR/KRW totals, assumptions, category rollups, contingency.

For a multi-topic request, synthesize these roles in one Codex answer unless the user explicitly asks to split files or simulate separate agents.

## Work Rules

- Respond to the user in Korean unless they ask otherwise.
- Keep travel-facing documents in Korean. English is acceptable for URLs, official names, and code identifiers.
- Before changing dates, prices, schedules, opening hours, ticket windows, or booking links, verify live information from official or primary sources. These facts are time-sensitive and can affect spending.
- Clearly mark assumptions and the date of verification in research or plan updates.
- Do not overwrite existing Claude-era research. Add revisions, update the appropriate version file, or create a new versioned artifact.
- If editing a dashboard, keep route data, calendar data, transport rows, budget rows, and visible tab labels consistent.
- If creating a new final dashboard version, update `plan/VERSIONS.md`. Because `index.html` is the current shared page, replace it only when the user wants to publish/share that version.
- Preserve user changes in the working tree. Do not reset or checkout files unless explicitly requested.

## Local Running And Verification

This is a static HTML project with no package install step.

Useful commands from `/Users/bell/Travel/italy/20260916-20260928`:

```bash
python3 -m http.server 8080
```

Then open:

- `http://localhost:8080/index.html`
- `http://localhost:8080/plan/italy-planner.html`

For quick file inspection:

```bash
rg --files
git status --short
```

When editing HTML, verify at least:

- page loads without JavaScript console errors,
- Leaflet map renders,
- course tabs and section tabs switch correctly,
- mobile width does not overlap text,
- map and route data match the selected course.

## Near-Term Planning Checklist

As of the Codex setup date, 2026-05-31, the next useful work is:

1. Decide which route family is the working favorite among Course A-D.
2. Re-verify live booking facts for flights, hotels, major attractions, intercity trains, ferries, Dolomites transport, and rental car assumptions.
3. Turn the selected course into a booking checklist with owner, official link, target booking date, refund policy, and backup option.
4. Update the dashboard and Markdown plan from the verified checklist.
5. Keep budget numbers synchronized across dashboard, Markdown, and any spreadsheet-derived source.
