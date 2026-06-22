# Clevedon Live

A single-page live dashboard for **Clevedon, Somerset** (with a built-in switch to **Orford, Suffolk**): live cameras, real tide data, weather, sun times and sea temperature, designed to sit full-screen on a wall or kitchen TV.

**Live site:** https://peterjbeard.github.io/ClevedonLive/

It is one self-contained `index.html` file. No build step, no framework, no API keys, no server. Open it on any modern browser and it runs.

---

## Features

- **Live cameras** with a large main view and two side-by-side picture-in-picture tiles. Tap any PiP to bring it to the main view, or turn on **Auto-cycle** to rotate through them automatically (every 20 seconds).
- **Live tides** from the Environment Agency tide-gauge network, with the next high and low water, a tide curve for the day, a spring/neap indicator and (for Clevedon) a Marine Lake overtopping flag.
- **Weather now** plus a **12-hour** strip and a **7-day** outlook, with rain probabilities.
- **Sun and water**: a live sunrise-to-sunset arc with a countdown to the next event, plus sea-surface temperature and the day's max UV.
- **Clevedon / Orford toggle** in the top bar that switches the whole board (cameras, tides, weather, sun) to that location.
- **Liveness indicator** with per-source "updated" timestamps, a manual refresh button, a full-screen button and a live clock with seconds.
- **Light, responsive design** that scales from a laptop to a 1080p TV, and remembers your chosen location and main camera between visits.

---

## Cameras

**Clevedon**

| View | Source |
| --- | --- |
| The Bay & Pier | Clevedon Sailing Club (ipcamlive) |
| Marine Lake | Clevedon Marine Lake Web Cam (YouTube live) |
| Pier &middot; Porthole Room | Clevedon Pier (ipcamlive) |

**Orford**

| View | Source |
| --- | --- |
| Camera 1 / Camera 2 | Orford (ipcamlive) |

### A note on the Marine Lake camera

The Marine Lake's only public feed is its YouTube live stream, and the channel owner has **disabled embedding** ("Error 153"). Because of that:

- **On the live web address** (this site, served over HTTPS) the Marine Lake tile shows the live frame with a **play button** — one tap plays the moving video. It cannot autoplay or run silently, so a single tap is required, the same as every other site that lists this camera.
- **If you open `index.html` as a local file** (a `file://` page), browsers block the YouTube embed entirely, so the tile falls back to a **near-live photo** (the stream's current frame, refreshed every ~20 seconds) plus a "Watch live on YouTube" button.

In short: host the page (as it is here) for the moving video; the photo fallback keeps it useful offline.

---

## Data sources

- **Weather, sun times, sea temperature** &mdash; [Open-Meteo](https://open-meteo.com/) (keyless, free for non-commercial use).
- **Tides** &mdash; [Environment Agency real-time flood-monitoring API](https://environment.data.gov.uk/flood-monitoring/doc/reference) (Open Government Licence). Clevedon uses the **Avonmouth** gauge (`E72639`); Orford uses the nearest gauge, **Harwich** (`E71439`).
- **Cameras** &mdash; ipcamlive players and the Clevedon Marine Lake YouTube channel.

### How the tide predictions work

The app pulls roughly 28 days of 15-minute measured levels from the live tide gauge and fits a small **harmonic model** (the main tidal constituents: M2, S2, N2, K1, O1 and the shallow-water overtides M4, MS4, M6) to it on every refresh. It then projects the next several days of high and low water from that fit. Because it is refitted from live data each time, the predictions track the real, currently-measured tide.

> **Not for navigation.** Tide times and heights are indicative only. Orford sits up the River Ore, so its true times run a little later than the Harwich gauge. Always use official sources for navigation or swimming-safety decisions, including the [Clevedon Marine Lake overtopping forecast](https://clevedonmarinelake.org.uk/visit-us/).

---

## Running it

- **Hosted (recommended):** it is published with GitHub Pages at the live link above. This is the version to use, because it is the only way the Marine Lake moving video will play.
- **Local:** double-click `index.html`. Everything works except the Marine Lake moving video, which falls back to the refreshing photo (a browser restriction on local files, not a bug).

### Updating the site

Edit `index.html`, then upload the new version to this repository (Add file &rarr; Upload files &rarr; commit to `main`). GitHub Pages redeploys automatically within a minute or so.

### Customising

Everything is driven by the `LOCATIONS` object near the top of the `<script>` in `index.html`. Each location defines its coordinates, its tide gauge (`eaMeasure`), its sea-temperature points and its list of cameras. To add a camera, add an entry to that location's `cams` array; to add a place, add a new location and a button to the top-bar toggle.

---

## Tech notes

- One self-contained HTML file: markup, CSS and vanilla JavaScript in a single document.
- No dependencies beyond two web fonts loaded from Google Fonts (with system-font fallbacks).
- All data comes from keyless, CORS-enabled public APIs, fetched client-side.
- Auto-refreshes weather, tides and the live photo on timers, reloads the camera streams periodically to recover from stalls, and reloads the whole page every few hours to stay fresh on an always-on display.

## Licence

Released under the MIT Licence (see `LICENSE`). Camera feeds, weather and tide data remain the property of their respective providers and are used under their terms.
