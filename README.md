# Find route

A skill that makes your agent look up journey times instead of guessing them. Ask how long it takes to walk, cycle or take public transport between two places, and it asks routing services and shows where each place resolved, so you can check the answer.

```text
$ route "WC1N 2AD" "SE1 8NY" --at "2026-10-17 17:30"
From: WC1N 2AD (postcode centroid)  [51.52501,-0.11740]
To:   SE1 8NY (postcode centroid)  [51.50577,-0.10480]
Straight line: 2.3 km
Leave at: 2026-10-17 17:30 BST
Walk       36 min   2.7 km  (OSM foot router)
Cycle      17 min   2.8 km  (OSM bike router)
Cycle      10 min   2.9 km  (TfL)
Transit    26 min        —  (TfL)
         dep 17:37, arr 18:03: Walk to Rosebery Avenue / Mount Pleasant → 63 bus to Stamford Street → …
```

Two London postcodes on a Saturday evening: walking and cycling from OpenStreetMap, plus TfL's bus route for that departure time.

## How it works

- **Walking and cycling, worldwide:** the free OpenStreetMap routers at routing.openstreetmap.de.
- **Public transport in London:** the free TfL Journey Planner.
- **Public transport elsewhere:** the Google Routes API, which needs your own key.
- **Places:** UK postcodes via postcodes.io, everything else via OpenStreetMap's Nominatim, or pass `lat,lng`.

## Requirements

- Python 3.9 or later. The script uses only the standard library.
- Optional: for public transport outside London, a Google Maps Platform API key with the Routes API enabled, set as `GOOGLE_MAPS_API_KEY`. Google bills it after a monthly free allowance.

The free services are meant for light use, so this suits trip planning, not bulk lookups.

## Installation

```bash
npx skills add HartreeWorks/skill--find-route
```

If you get "command not found", [install Node](https://github.com/HartreeWorks/skills/blob/main/how-to-install-node.md) then try again.

## Try it

Ask your agent:

```text
How long does it take to get from King's Cross station to the Tate Modern on foot, by bike and by public transport, leaving at 6pm on Saturday?
```

You get walking, cycling and public-transport times with distances, the public-transport route, and the source of each figure.

## Updates

At most once a day, the skill checks for a new release. When there is one, your agent shows what changed, reviews the code changes, and asks whether to update now, update automatically from now on, or not now. Automatic updates wait three days after a release first appears. To stop the checks, ask your agent to run `scripts/check-update.sh --off`.

## Documentation

See [SKILL.md](./SKILL.md) for complete documentation and usage instructions.

## About

Created by [Peter Hartree](https://x.com/peterhartree) of [AI Wow](https://wow.pjh.is).

Find more skills at [HartreeWorks/skills](https://github.com/HartreeWorks/skills).
