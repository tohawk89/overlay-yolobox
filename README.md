# Masjid-Yolobox Overlay System

A real-time lower-third overlay system designed for YoloBox live streams, powered by Firebase Realtime Database.

## Overview

This project provides a simple way to manage and display speaker names (penceramah) on a live stream.

### Components

| File | Purpose |
|---|---|
| `overlay.html` | Renderer page — add as a Web URL source in YoloBox/OBS at 1920×1080. Reads Firebase and auto-shows/hides the lower-third banner based on the schedule. |
| `control.html` | Admin dashboard — use on any phone, tablet, or laptop to manage the overlay in real time. Password protected. |
| `guide.html` | End-user guide — explains how to use the control panel during a live broadcast. Linked from `control.html`. |
| `firebase-config.js` | Local Firebase credentials (git-ignored). Copy from `firebase-config.example.js`. |

### How the Overlay Works

1.  **Schedule-driven**: Upload a CSV via `control.html`. When a session's start time arrives, the banner appears automatically showing the program name and speaker. When the session ends, the banner hides automatically — within 10 seconds.
2.  **Manual override**: During active sessions, the operator can edit the name, hide/show the banner, or "Jump to this speaker" from the schedule list. Changes take effect instantly.
3.  **Surprise speakers**: During gaps between sessions, showing the overlay manually is supported. The banner stays visible until the next scheduled session takes over.

## Firebase Data Structure

```
ceramic_overlay/
  isVisible       boolean   — whether the banner is currently shown
  name            string    — speaker name (bottom black bar)
  title           string    — program name (top white bar)
  lastSessionKey  string|null — tracks the last active session key ("DD/MM/YYYY_HH:MM:SS") to detect session-end transitions across page reloads
  updatedAt       number    — Unix timestamp of last change

ceramic_schedule/
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
3.  **Usage**:
    -   Open `control.html` in your browser and unlock the dashboard using the password configured in `firebase-config.js` (default is **`admin`**).
    -   Load `overlay.html` as a Web URL Source in your streaming software (YoloBox/OBS) at 1920×1080 resolution.
    -   Upload your schedule CSV before the event begins.

## Technologies

-   HTML5 / CSS3
-   JavaScript (Firebase SDK v10)
-   Firebase Realtime Database
-   Firebase Hosting
