# TO-LD — Archer TO/LD performance calculator

Static site on GitHub Pages: https://abell405.github.io/TO-LD/

Deployed by `.github/workflows/pages.yml` (Pages source must be "GitHub Actions"). It runs on every push to `main` and every 10 minutes, fetching the latest METAR for the airports in `METAR_IDS` from aviationweather.gov into `metar.json` beside `index.html`. aviationweather.gov blocks browser (CORS) requests, which is why the fetch happens in the workflow. To add METARs for another airport, add its ICAO to `METAR_IDS` (comma-separated).

## Always push changes
After ANY change to this project (code, airport data, settings, README), commit and push to `origin main` right away, without asking. GitHub Pages redeploys from `main`, so a push is how a change goes live. Tell the user it will be live at the URL above in a minute or two.

## Project layout
- `index.html` is the whole app: HTML, CSS and JS in one file, no build step, no backend.
- `AIRPORTS` (near the top of the script) holds airport and runway data: field elevation, and per runway end the magnetic heading, takeoff length (TORA) and landing length (LDA). A runway with `noAuto:true` is never chosen by the METAR runway auto-pick (KPGD 9/27, not primary runways) but can still be picked by hand.
- `DATA` holds the digitized PA-28-181 POH Section 5 chart data (Report VB-2960). Don't edit it unless re-digitizing the charts.
- The page fetches the METAR live from api.weather.gov (NWS, allows browser requests) every 2 minutes and on return to the tab; `metar.json` is the backup. The newest report from either source wins. GitHub's scheduler only runs the workflow every 40–60 min, so don't rely on it for freshness.
- "Paste a METAR" (under the METAR card, also shown in Compact view) parses a pasted METAR/SPECI and applies it the same way as "Use this METAR" (OAT, altimeter, wind, runway pick); a different station ID switches the ICAO.
- `metar.json` is not in the repo; the workflow creates it at deploy time. The page reads it, shows the METAR, and the "Use this METAR" button fills OAT, altimeter and wind exactly as the METAR reports it (no true-to-magnetic conversion, by the user's choice) and picks the runway most into the wind.
- Density altitude formula (keep as is): DA = PA + 120 × (OAT − ISA), with ISA = 15 − 2 × PA/1,000.
- `VO` is the maneuvering-speed (Vo) by weight table. `SET_DEF` holds the default settings (Vr 60, Vy 76, Vx 64, etc.).
- Chart distances (T.O./Ldg roll and over 50 ft) round to the nearest 100 ft (`r100`); the wet ×1.5 result rounds the same way before the 1,000 ft aimpoint is added.
- `LIMITS` holds the runway and wind limits. Form rules: T.O. Distance = T.O. ground roll; T.O. Distance Required = T.O. roll + Ldg roll, or 2,000 ft if longer; Ldg 50ft Obstacle and Ldg Distance = over-50-ft chart with no headwind credit (tailwind still counts); Ldg Distance Required = Ldg Distance × 1.5 only when Wet, then + 1,000 ft aimpoint; Runway Length Required = larger of T.O. and Ldg Distance Required; Maneuvering Speed = Vo at landing weight. All runways are hard surfaced (Runway Condition always "Hard"; the input is a Dry/Wet switch). Runway width ≥ 50 ft; max tailwind 10 kt; max surface wind 30 kt incl. gusts.

## Checks before pushing
- Keep the chart math unchanged unless asked. It matches the POH printed examples within 0–2.5% (long side).
- Syntax-check the script after edits (e.g. extract the `<script>` body and run it through `node`).

## Keep it generic
Don't add any organization or business names anywhere in this project (page text, code comments, README, commit messages). Keep limits as plain numbers.
