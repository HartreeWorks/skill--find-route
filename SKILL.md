---
name: find-route
description: Travel time and distance between two places by walking, cycling or public transport, from routing services. Use for any journey-time claim—"how long to walk/cycle/get from X to Y", minutes from a hotel to a venue, station or office—instead of estimating.
---

# Find route

A journey time in a report comes from this tool, never from memory or a straight-line guess.

## Before starting: check for updates

If `scripts/check-update.sh` exists in this skill's folder, run `bash <this skill's folder>/scripts/check-update.sh` from the user's project folder. It checks at most once a day and prints nothing when the skill is current.

- **It reports an update:** run it with `--diff` and read the code changes. Show the user the release notes and a short summary of the changes, flagging anything the notes do not explain. Ask whether to update now, update now and automatically from now on, or not now. For the first run it with `--apply`; for the second run it with `--enable-auto`, then `--apply`; for the third carry on.
- **It reports an automatic update:** run it with `--diff` and read the changes. If they match the release notes and do nothing risky, run it with `--apply` and tell the user in one line. Otherwise show the user your concerns and ask.
- **After `--apply` succeeds:** re-read this SKILL.md before continuing.

## Looking up a route

Run `~/.agents/skills/find-route/scripts/route "FROM" "TO"` (`--help` for options). Places can be a UK postcode, an address with city and country, or `lat,lng`.

- `--mode walk|cycle|transit|all` (default `all`).
- `--at "2026-10-17 17:30"` sets the departure time, and `--arrive` makes it the arrival time. Always pass it for transit, which otherwise plans for now. Outside London add `--tz Europe/Paris` (or the right zone).

## Reading the output

- **Check the resolved places.** The first two lines show where each input landed. Bare names can resolve to the wrong country ("Oxford station" found Ontario), and the script warns when the two points are over 300 km apart. A postcode resolves to its centroid, which is fine for walking times but can be 100 m or more from the door.
- **Walk and cycle** come from the OpenStreetMap routers (about 5 km/h walking and 15 km/h cycling). In London the script also asks TfL for a cycle time, which assumes a faster rider and runs about a third quicker. Quote the range.
- **Transit in London** comes from the TfL Journey Planner, timed to `--at`, with the legs listed. TfL plans walk-only trips poorly, so walk times come from OSM.
- **Transit elsewhere** uses the Google Routes API. The key comes from `GOOGLE_MAPS_API_KEY`; without it, transit outside London is unavailable. If Google reports an error or no key is set, say so and fall back to Google Maps directions in Chrome. Do not change the key or the project's settings yourself.

When reporting, give the time, the mode and the source ("36 min walk, OSM"), plus the hire-bike docking time where it matters.

## Limits

The OSM routers and Nominatim are free public services meant for light use: a few dozen lookups per task, not bulk loops. Nominatim allows one request a second.
