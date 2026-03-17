# JogTracker

A single-file web app for tracking runs with live GPS, route maps, history, stats, and weekly goals.

**Live app:** https://timothycareyevans.github.io/jog-tracker/

## Features

- Live GPS tracking with Leaflet map and route polyline
- Real-time HUD: time, distance, pace
- Run history with expandable route maps
- Stats charts: weekly distance, pace trend
- Weekly goals with progress bars
- Cloud sync via Supabase (optional — works offline with localStorage)

## Supabase Setup (optional)

1. Create a free project at [supabase.com](https://supabase.com)
2. Go to **SQL Editor** and run:

```sql
create table runs (
  id uuid primary key default gen_random_uuid(),
  device_id text not null,
  date timestamptz default now(),
  duration_seconds int,
  distance_meters float,
  avg_pace_sec_per_km float,
  coordinates jsonb,
  notes text
);
```

3. Open the app — on first launch enter your **Project URL** and **Anon Key** from:
   Supabase dashboard → Settings → API

> **No Supabase?** Just click "Skip" on the setup modal. All runs will be saved to your browser's localStorage.

## Stack

- Plain HTML/CSS/JS (single file, no build step)
- [Leaflet.js](https://leafletjs.com) — interactive maps
- [Chart.js](https://www.chartjs.org) — stats charts
- [Supabase JS v2](https://supabase.com/docs/reference/javascript) — cloud storage
- Browser Geolocation API — live GPS via `watchPosition()`
