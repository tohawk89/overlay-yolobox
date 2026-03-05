# Masjid-Yolobox Overlay System

A real-time lower-third overlay system designed for YoloBox live streams, powered by Firebase Realtime Database.

## Overview

This project provides a simple way to manage and display speaker names (penceramah) on a live stream. It consists of two main components:

-   **`overlay.html`**: The renderer page. This should be added as a "Web URL" source in your YoloBox or OBS. It listens for changes in Firebase and displays the overlay.
-   **`control.html`**: The control dashboard. Use this on a separate device (phone, tablet, or laptop) to update the name and toggle the overlay visibility.

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
    -   Open `control.html` in your browser (supports local file double-clicking) and unlock the dashboard using the password configured in `firebase-config.js` (default is **`admin`**).
    -   Load `overlay.html` as a Web URL Source in your streaming software (YoloBox/OBS) at 1920x1080 resolution.

## Technologies

-   HTML5 / CSS3
-   JavaScript (Firebase SDK v10)
-   Firebase Realtime Database
