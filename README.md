# Clevedon Live

A single-page live dashboard for **Clevedon, Somerset** (with a built-in switch to **Orford, Suffolk**): live cameras, real tide data, weather, sun and moon times, sea temperature, air quality, pollen and a rain radar. Designed to sit full-screen on a wall or kitchen TV.

**Live site:** https://peterjbeard.github.io/ClevedonLive/

It is one self-contained `index.html` file. No build step, no framework, no API keys, no server. Open it on any modern browser and it runs.

---

## Features

**Cameras**
- A large main view with two side-by-side picture-in-picture tiles. Tap a PiP to bring it to the main view.
- **Auto-cycle** rotates the main camera every 20 seconds.
- **Full view** with **zoom and pan**: open any camera full-screen, zoom in with the +/− controls (or scroll), and drag to scan around for detail. Zoom is a digital crop, so it softens as you go in. Controls and Close sit along the bottom.
- **Cinema mode** rotates the cameras full-screen with a minimal clock and a live data ribbon.

**Weather, sun, sky**
- Current conditions, a 12-hour strip and a 7-day outlook with rain probabilities.
- Sunrise-to-sunset arc with a live countdown, day length and change versus yesterday, and the golden-hour window.
- The whole background **follows the real sun** through dawn, day, golden hour, dusk and a starry night.

**Tides**
- Live tide-gauge level with next high and low water, a tide curve for the day, a spring/neap indicator and (for Clevedon) a Marine Lake overtopping flag.
- An **Upcoming tides** table of the next few days of highs and lows.

**Conditions (facts only, no advice)**
- **Wind**: speed, direction, Beaufort force, gusts and knots, on a compass dial.
- **Moon**: phase, percent illuminated and the next full moon.
- **Air quality**: European AQI with its standard band, plus PM2.5 and PM10.
- **Pollen**: grass, tree and weed levels.
- **Pressure**: current value with rising/falling/steady.

**Rain radar**
- A live animated precipitation map (RainViewer) centred on the chosen town, on an OpenStreetMap base.

**Display and kiosk**
- **Clevedon / Orford toggle** switches the whole board for that location.
- **Keep-awake** holds a screen wake-lock so a TV or tablet does not sleep.
- A live clock with seconds and a friendly greeting, a one-line "right now" summary (conditions, temperature, wind and the next tide), a liveness indicator with per-source "updated" times, manual refresh, and full-screen.
- **Fits the screen with no scrolling.** The board scales to fill a TV and shrinks to fit smaller screens or any browser zoom, so the whole dashboard is always visible without a scrollbar.

---

## Controls

| Control | What it does |
| --- | --- |
| Clevedon / Orford (top bar) | Switch the whole board to that location |
| Auto-cycle (on the camera) | Rotate the main camera every 20s |
| Full view (on the camera) | Full-screen one camera with zoom (+/−), pan (drag) and Close, all along the bottom |
| Radar button (top bar) | Open the live rain-radar map |
| Cinema button (top bar) | Full-screen rotating cameras with a data ribbon |
| Full screen button (top bar) | Browser full-screen |
| Refresh button (top bar) | Refresh all data now |

Your location and main camera are remembered between visits.

---

## Cameras

**Clevedon:** The Bay & Pier (Sailing Club, ipcamlive) · Marine Lake (YouTube) · Pier Porthole Room (ipcamlive).
**Orford:** two ipcamlive views.

### A note on the Marine Lake camera

The Marine Lake's only public feed is its YouTube live stream, and the channel owner has **disabled embedding** ("Error 153"). So:

- **On the live web address** (this site, over HTTPS) the Marine Lake plays the live video with a one-tap play button.
- **Opened as a local file** (`file://`), browsers block the YouTube embed, so it falls back to a **near-live photo** (the stream's current frame, refreshed every ~20s) with a "Watch live on YouTube" button.

Host the page (as it is here) for the moving video.

---

## Data sources

- **Weather, sun, day length, air quality, pollen** — [Open-Meteo](https://open-meteo.com/) (keyless).
- **Tides** — [Environment Agency real-time flood-monitoring API](https://environment.data.gov.uk/flood-monitoring/doc/reference) (Open Government Licence). Clevedon uses the **Avonmouth** gauge (`E72639`); Orford uses the nearest gauge, **Harwich** (`E71439`).
- **Rain radar** — [RainViewer](https://www.rainviewer.com/), on an [OpenStreetMap](https://www.openstreetmap.org/copyright) base via Leaflet.
- **Cameras** — ipcamlive players and the Clevedon Marine Lake YouTube channel.

### How the tide predictions work

The app pulls roughly 28 days of 15-minute measured levels from the live gauge and fits a small **harmonic model** (M2, S2, N2, K1, O1 plus the shallow-water overtides M4, MS4, M6) on every refresh, then projects the next several days of high and low water. Because it is refitted from live data, the predictions track the real, currently-measured tide.

> **Not for navigation.** Tide times and heights are indicative only. Orford sits up the River Ore, so its true times run a little later than the Harwich gauge. Use official sources for navigation or swimming-safety decisions, including the [Clevedon Marine Lake overtopping forecast](https://clevedonmarinelake.org.uk/visit-us/).

---

## Running and hosting

- **Hosted (recommended):** published with GitHub Pages at the live link above. This is the version to use, as it is the only way the Marine Lake moving video plays.
- **Local:** open `index.html`. Everything works except the Marine Lake moving video, which falls back to the refreshing photo.

The layout **fits the screen with no scrolling**: it scales to fill a TV and shrinks to fit smaller screens or any browser zoom, so the whole board is always visible and nothing is clipped.

### Updating the site

Edit `index.html`, then upload it to this repository (Add file → Upload files → commit to `main`). GitHub Pages redeploys within a minute or so.

### Customising

Everything is driven by the `LOCATIONS` object near the top of the `<script>` in `index.html`. Each location defines its coordinates, tide gauge (`eaMeasure`), sea-temperature points and cameras. Add a camera to a location's `cams` array, or add a whole new place with its own toggle button.

## Licence

Released under the MIT Licence (see `LICENSE`). Camera feeds, weather, tide and map data remain the property of their respective providers and are used under their terms.
