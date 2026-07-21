---
name: deutschland-hiking-planner
description: Plan and document day hikes in Germany reachable with the Deutschlandticket. Use when a user wants to save their home city, discover or compare ticket-accessible hikes, receive a route recommendation, create a printable map or offline navigation kit with landmarks and GPX, or keep a history of completed hikes.
---

# Deutschland Hiking Planner

Plan a realistic hike from the user's remembered city, verify current local-transit access, and maintain a small local log of completed hikes.

## Local state

Use `scripts/hike_state.py show` at the start of a planning or history request. The state file is local at `$CODEX_HOME/hiking-planner/state.json` (or `~/.codex/hiking-planner/state.json` if `CODEX_HOME` is unset).

- If `city` is absent, ask: “Which city should I use as your starting point?” Do not infer it from account details or a past conversation.
- After the user answers, run `set-city` with their exact city name. State that it is saved locally and can be changed or erased on request.
- Read `completed_hikes` before recommending routes; avoid repeating them unless the user asks to revisit one.
- Record a hike only when the user says they completed it or asks to add it. Do not log ideas, recommendations, or bookings as completed hikes.
- Use `delete-city` or `delete-hike ID` immediately when asked to forget information.

## Find and recommend hikes

Gather only the choices needed to make a safe recommendation: target date or season, time available, desired distance/difficulty, and whether a long journey is acceptable. If the user has not specified them, make a reasonable beginner-friendly default and say so.

Research live information before making an eligibility claim:

1. Find trail candidates with reliable route details (distance, elevation, official trail information, route geometry, hazards or closures).
2. Check the current transit journey from the saved city to both trail endpoints. Verify the relevant trains and buses are included in the Deutschlandticket; do not treat ICE, IC, EC, long-distance buses, or privately operated exceptions as eligible. Check the official Deutschlandticket rules and the current timetable/operator information.
3. Confirm that the return journey works after a realistic hiking duration. Flag reservation requirements, replacement buses, seasonal service, last-connection risk, and any uncertain fare boundary.
4. Prefer a route that starts and ends near transit stops, has a clear waymark or published track, fits daylight and the user's level, and is not already in their completed history.

Propose one best route, with one or two alternatives only if they meaningfully differ. For every proposal, show distance, ascent, estimated hiking time, travel time each way, start/end stops, service types, why the ticket is expected to cover it, expected return margin, and the sources checked. Clearly separate verified facts from assumptions. Never promise that a connection will still run at departure time; tell the user to recheck shortly before travel.

## Create an offline navigation kit

Create the kit only after the user selects a route (or explicitly asks for a map for a named route). Obtain the published GPX from the route owner first. If no authoritative track exists, say so and ask whether the user wants a clearly labelled reconstructed track; do not present it as official.

Deliver both a printable PDF and the route GPX. The GPX must retain its source and retrieval date in metadata when possible. Do not substitute a map image for a usable track.

Before building the map, collect current local data around the route from authoritative sources and, where appropriate, OpenStreetMap. Include only features useful on foot:

- route junctions and decision points; named peaks, lakes, streams, huts, shelters, viewpoints, and trail markers;
- public-transport stops, parking/start points, water refill points, toilets, food, and emergency-access points when verified;
- closures, unstable terrain, river crossings, steep sections, private-property boundaries, and areas without mobile coverage when reliably known.

Never invent a landmark, amenity, opening time, or water source. Distinguish `verified`, `seasonal`, and `unverified` features in the legend. Do not make an unsafe feature (for example a seasonal tap) part of the route plan without confirmation.

Use the `pdf` skill to make a north-up, print-friendly A4 PDF. It must include:

- title, date, route start/end, distance, ascent/descent, hiking-time estimate, difficulty, and track-source date;
- a detailed route map with the highlighted track, route direction arrows, labelled landmarks, numbered decision points, start/finish, and transit markers;
- a coordinate grid, north arrow, scale bar, latitude/longitude for start, finish, and emergency meeting points, plus a map-sheet note such as "WGS84 coordinates";
- a compact turn/landmark table: point number, coordinate, distance from start, instruction, and next reliable landmark;
- water, food, toilet, shelter, emergency, hazard, and "no signal" symbols with a legible legend;
- a transit summary, last safe return, safety advice, sources, map-data attribution, retrieval date, and a QR code or short link to the downloaded GPX.

Design for offline use: ensure every critical navigation fact is present on the paper PDF and in the GPX; keep labels readable in black-and-white printing; do not require an online map, app, or a URL to follow the route. A QR code and links are conveniences, not navigation dependencies.

Render the finished PDF and inspect it before delivery. Check that route segments are continuous, labels do not cover decision points, the scale and coordinate grid are legible, source attribution is visible, and every listed amenity lies plausibly near the track. Provide the PDF and GPX as user-facing outputs, along with a short note of any unverified or seasonal features.

## Log a completed hike

When the user confirms they went, run `add-hike`. Record at least the route title and date; include distance, ascent, endpoints, and notes when known. Read back the saved entry briefly. Use `show` to answer history questions.

## State helper

```text
python3 scripts/hike_state.py show
python3 scripts/hike_state.py set-city "Freiburg im Breisgau"
python3 scripts/hike_state.py add-hike --title "Belchen summit loop" --date 2026-07-21 --distance-km 12.4 --ascent-m 750 --start "Münsterplatz" --finish "Münstertal" --notes "Sunny; bring water"
python3 scripts/hike_state.py delete-city
python3 scripts/hike_state.py delete-hike 1
```
