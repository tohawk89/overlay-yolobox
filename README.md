# Masjid-Yolobox Overlay System

A real-time lower-third overlay system designed for YoloBox live streams, powered by Firebase Realtime Database.

## Overview

This project provides a simple way to manage and display speaker names (penceramah) on a live stream.

### Components

| File | Purpose |
|---|---|
| `overlay.html` | Renderer page — add as a Web URL source in YoloBox/OBS at 1920×1080. Reads Firebase and updates the lower-third banner automatically based on the schedule. The banner **always stays visible**; only the text changes. |
| `control.html` | Admin dashboard — use on any phone, tablet, or laptop to manage the overlay in real time. Password protected. |
| `guide.html` | End-user guide — explains how to use the control panel during a live broadcast. Linked from `control.html`. |
| `firebase-config.js` | Local Firebase credentials (git-ignored). Copy from `firebase-config.example.js`. |
| `database.rules.json` | Firebase Realtime Database security rules — deployed via `firebase deploy`. |

### How the Overlay Works

1.  **Schedule-driven**: Upload a CSV via `control.html`. When a session's start time arrives, the banner automatically shows the program name and speaker — within 10 seconds.
2.  **2-hour grace period**: After a session ends, the last speaker's info stays on the banner for up to **2 hours**. After 2 hours with no new session, the banner text clears automatically.
3.  **Manual override**: During active sessions, the operator can edit the name or "Jump to this speaker" from the schedule list. Changes take effect instantly via `SAVE CHANGES`.
4.  **Surprise speakers**: During gaps between sessions, typing a name and pressing `SAVE CHANGES` shows it on the banner immediately. The next scheduled session will take over automatically.

## Firebase Data Structure

```
ceramah_overlay/
  isVisible       boolean     — reserved (banner always shown; toggle logic commented out for future use)
  name            string      — speaker name (bottom black bar)
  title           string      — program name (top white bar)
  lastSessionKey  string|null — tracks the last active session key ("DD/MM/YYYY_HH:MM:SS") to detect session-end transitions
  sessionEndedAt  number|null — Unix timestamp of when the last session ended; used to enforce the 2-hour grace period
  updatedAt       number      — Unix timestamp of last change

ceramah_schedule/
  [array of session objects]
    startDate     string    — "DD/MM/YYYY"
    startTime     string    — "HH:MM:SS"
    endDate       string    — "DD/MM/YYYY"
    endTime       string    — "HH:MM:SS"
    subject       string    — program name
    description   string    — speaker name
```

## Setup

1.  **Firebase Project**:
    -   Create a project at [https://console.firebase.google.com/](https://console.firebase.google.com/).
    -   Enable **Realtime Database**.
2.  **Configuration**:
    -   Rename `firebase-config.example.js` to `firebase-config.js`.
    -   Open `firebase-config.js` and update the `window.firebaseConfig` object with your project's `apiKey`, `databaseURL`, and `projectId`.
    -   You can also configure your custom dashboard password using the `adminPassword` property.
    -   *(Note: `firebase-config.js` should be ignored by Git to keep your specific configuration separated.)*
3.  **Deploy**:
    -   Run `npx firebase-tools deploy` to publish hosting and push database security rules.
4.  **Usage**:
    -   Open `control.html` in your browser and unlock the dashboard using the password configured in `firebase-config.js` (default is **`admin`**).
    -   Load `overlay.html` as a Web URL Source in your streaming software (YoloBox/OBS) at 1920×1080 resolution.
    -   Upload your schedule CSV before the event begins.

## Technologies

-   HTML5 / CSS3
-   JavaScript (Firebase SDK v11.6.0)
-   Firebase Realtime Database
-   Firebase Hosting
