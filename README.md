# Tesla Telemetry — Live Drive Visualizer

A single-file web app (`index.html`, zero dependencies, no build step) that turns the
Tesla Model 3 in-car browser into a live telemetry dashboard: speed, altitude,
acceleration, g-force, distance, heading and more, drawn as dozens of gauges,
charts, histograms, and event lists.

## What it shows

- **Status bar:** data source (LIVE GPS / SIMULATOR / NO SIGNAL), last-fix age,
  GPS accuracy, fix count, units toggle (mph+ft / km/h+m), Start / Pause /
  Reset, Export trip (CSV), and a one-tap Simulator.
- **15 hero readouts:** speed, altitude, vertical speed, acceleration (g),
  peak deceleration, distance, trip time, average and max speed, heading,
  latitude, longitude, elevation gain/loss, current g-force.
- **34 visualization panels:** arc speedometer and altitude gauges, time-series
  charts (speed, altitude, acceleration, vertical speed, distance, heading,
  elevation vs distance, cumulative elevation, GPS accuracy), g-force circle,
  compass rose, breadcrumb route trail, three histograms, speed-band time split,
  trip stats, 0–60 mph timer, braking and acceleration event lists, max-speed
  marker, last-60-seconds speed ribbon, two scatter plots, g-force gauge,
  heading distribution rose, trip timeline, position/fix-quality panel, vertical
  speed gauge, speed trend sparkline, stop log, and speed percentiles.
- **Overlay system:** every time-series chart has an overlay picker (altitude,
  acceleration, heading, distance, vertical speed, …) that draws a second series
  on a shared time axis with its own scale, plus a short "why this overlay"
  tip explaining what the combination reveals.

## Deploy on Vercel (static site)

1. Put this folder in a Git repo (or drag the folder into the Vercel dashboard
   as a new project).
2. Framework preset: **Other**. Build command: **(leave empty)**. Output
   directory: **the repo root** (where `index.html` lives).
3. Deploy. Vercel serves `index.html` at the project URL — open that URL in
   the Tesla browser. No server, no API keys, no account needed.

## Honest limits in the Tesla browser

- **GPS updates arrive roughly once every 30 seconds** and positions are
  quantized (jumpy). Speed, acceleration, deceleration and g-force are all
  *derived* from those sparse fixes (haversine distance / time, speed deltas),
  so charts move in steps on real GPS. Raw fix markers are drawn on every
  chart so you can see what is measured vs interpolated.
- **GPS altitude is frequently null** in the car browser. Altitude-dependent
  panels show a dash when no altitude is reported.
- **No gyroscope, no accelerometer feed.** `DeviceMotionEvent` fires nothing
  in the Tesla browser; the app probes for it and reports its status honestly.
  Lateral g is derived from heading-change rate × speed.
- **No geocoding, no map tiles.** The route "map" is a self-drawn canvas
  breadcrumb trail of your own lat/lon points — it works fully offline.
- **Parked in a garage, everything reads zero** — that is correct behavior.
  Use the **Simulator** (clearly labeled, same data pipeline as real GPS) to
  preview every visualization anywhere: it generates realistic driving with
  acceleration, cruising, braking, turns, hills, and stops.

## Privacy

Nothing leaves the page. No network calls at all. The only thing persisted is
your units preference, in `localStorage`.
