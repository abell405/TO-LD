# Archer TO/LD Performance

A takeoff and landing performance calculator for the Piper PA-28-181 Archer. Enter the airport, runway, weather and weights, and it fills in every field of a TO/LD performance form.

**Live site:** https://abell405.github.io/TO-LD/

## What it does

- Computes takeoff ground roll and takeoff over 50 ft (flaps 0° or 25°), and landing ground roll and landing over 50 ft.
- Corrects for pressure altitude, temperature, weight, and headwind or tailwind, the same way the POH charts do.
- Works out pressure altitude, density altitude, and headwind/tailwind and crosswind components.
- Runway Length Required is the longer of the takeoff and landing distances. T.O. and Ldg Distance Required are the chart distances, with no added margin.
- Checks wind limits (17 kt crosswind, 10 kt tailwind, 30 kt surface wind including gusts).
- Adds a soft-field allowance (adjustable in Settings).
- Shows maneuvering speed from a Vo-by-weight table, plus Vr / Vy / Vx.
- Flags a short runway, a tailwind, too much crosswind, overweight, and conditions outside the charts.
- Remembers your last inputs on your own device. Nothing is sent anywhere.

## Data sources

- **Performance:** PA-28-181 POH Section 5, Report VB-2960 (Figures 5-7, 5-9, 5-11, 5-13, 5-41, 5-43). The charts were digitized and checked against each chart's printed example. All six land within 0–2.5% of the POH, on the long (conservative) side, and results round up to the next 10 ft.
- **KPGD runway data:** FAA-derived airport data, October 2026. Check it against the current Chart Supplement.

These are planning numbers with the same limits as reading the charts by hand: paved, level, dry runway, POH technique, full throttle before brake release. Always use your judgment and the POH.

## Adding an airport

Open `index.html`, find the `AIRPORTS` list near the top of the script, and add an entry shaped like KPGD's. Use one line per runway end, with the magnetic heading, the takeoff length (TORA) and the landing length (LDA) in feet:

```js
KPGD:{elev:26,name:"Punta Gorda",rwys:{"4":{hdg:35,tora:7193,lda:7193}, ...}},
KXXX:{elev:FIELD_ELEV,name:"Airport name",rwys:{"RWY":{hdg:MAG_HDG,tora:TAKEOFF_FT,lda:LANDING_FT}, ...}}
```

Commit the change and the site updates within a minute or two.
