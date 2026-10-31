# Archer TO/LD Performance

A takeoff and landing performance calculator for the Piper PA-28-181 Archer. Enter the airport, runway, weather and weights, and it fills in every field of a TO/LD performance form.

**Live site:** https://abell405.github.io/TO-LD/

## What it does

- Computes takeoff ground roll and takeoff over 50 ft (flaps 0° or 25°), and landing ground roll and landing over 50 ft, corrected for pressure altitude, temperature, weight and wind the way the POH charts do.
- Works out pressure altitude, density altitude, and headwind/tailwind and crosswind components.
- Fills the form fields this way:
  - **T.O. Distance** = takeoff ground roll.
  - **T.O. Distance Required** = takeoff ground roll + landing ground roll, or 2,000 ft if that is longer.
  - **Ldg Distance** = landing distance over 50 ft, with no headwind credit (a tailwind still counts).
  - **Ldg Distance Required** = Ldg Distance, +50% when the runway is wet, + 1,000 ft aimpoint.
  - **Runway Length Required** = the larger of T.O. and Ldg Distance Required.
  - **Maneuvering Speed** = Vo at landing weight (most conservative).
- Checks the required distances against the runway, plus crosswind (17 kt), tailwind (10 kt) and surface wind (30 kt including gusts) limits.
- Remembers your last inputs on your own device. Nothing is sent anywhere.

## Data sources

- **Performance:** PA-28-181 POH Section 5, Report VB-2960 (Figures 5-7, 5-9, 5-11, 5-13, 5-41, 5-43). The charts were digitized and checked against each chart's printed example. All six land within 0–2.5% of the POH, on the long (conservative) side, and distances are rounded to the nearest 100 ft.
- **KPGD runway data:** FAA-derived airport data, October 2026. Check it against the current Chart Supplement.

These are planning numbers with the same limits as reading the charts by hand: paved, level, dry runway, POH technique, full throttle before brake release. Always use your judgment and the POH.

## Adding an airport

Open `index.html`, find the `AIRPORTS` list near the top of the script, and add an entry shaped like KPGD's. Use one line per runway end, with the magnetic heading, the takeoff length (TORA) and the landing length (LDA) in feet:

```js
KPGD:{elev:26,name:"Punta Gorda",rwys:{"4":{hdg:35,tora:7193,lda:7193}, ...}},
KXXX:{elev:FIELD_ELEV,name:"Airport name",rwys:{"RWY":{hdg:MAG_HDG,tora:TAKEOFF_FT,lda:LANDING_FT}, ...}}
```

Commit the change and the site updates within a minute or two.
