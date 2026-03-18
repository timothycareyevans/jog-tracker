# JogTracker

A single-file web app for tracking runs with live GPS, route maps, history, stats, and weekly goals.

**Live app:** https://timothycareyevans.github.io/jog-tracker/

## Features

- Live GPS tracking with Leaflet map and route polyline
- Real-time HUD: time, distance, pace
- Run history with expandable route maps
- Stats charts: weekly distance, pace trend
- Weekly goals with progress bars
- Cloud sync via Firebase Firestore (optional — works offline with localStorage)

## Firebase Setup (optional)

### 1. Create a Firebase project

1. Go to [firebase.google.com](https://firebase.google.com) → **Add project**
2. Name it anything, disable Google Analytics if you like

### 2. Enable Anonymous Authentication

Firebase console → **Build → Authentication → Sign-in method** → enable **Anonymous**

### 3. Enable Firestore

Firebase console → **Build → Firestore Database** → **Create database** → choose a region → start in **production mode**

### 4. Set Firestore security rules

In Firestore → **Rules** tab, replace the default rules with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/runs/{runId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Click **Publish**.

### 5. Get your credentials

Firebase console → **Project Settings** (gear icon) → **General** → scroll to **Your apps** → click **</>** (Web) to register an app → copy:
- **API Key** (`apiKey`)
- **Project ID** (`projectId`)

### 6. Connect the app

Open JogTracker, enter your **API Key** and **Project ID** when prompted, click **Connect**.

> **No Firebase?** Click **Skip** — all runs save to your browser's localStorage automatically.

## Data structure

Runs are stored as Firestore documents at:
```
users/{anonymousUid}/runs/{runId}
```

Each document contains: `date`, `duration_seconds`, `distance_meters`, `avg_pace_sec_per_km`, `coordinates` (array of `{lat, lng, ts}`), `notes`.

## Stack

- Plain HTML/CSS/JS (single file, no build step)
- [Leaflet.js](https://leafletjs.com) — interactive maps
- [Chart.js](https://www.chartjs.org) — stats charts
- [Firebase JS SDK v10](https://firebase.google.com/docs/web/setup) — Firestore + Anonymous Auth
- Browser Geolocation API — live GPS via `watchPosition()`
