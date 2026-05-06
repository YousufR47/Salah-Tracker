# Salah Tracker Owner's Guide

This folder contains a self-contained Salah Tracker app. It runs in a browser, stores your salah data on your own device, and can work offline after it has been opened once through a proper browser URL.

This guide is written for a non-coder. It explains what the files do, how the app stores data, what was checked, what was improved for data safety, and what to do if something goes wrong later.

## Quick Start

The app folder is:

`C:\Users\yrehm\Downloads\salah-tracker`

The main app file is:

`index.html`

For simple use, you can double-click `index.html`.

For the best install/offline behaviour, open it through a local server. During testing, this URL worked:

`http://127.0.0.1:4174/index.html`

If Codex or another helper starts a small local server for this folder, use the URL they give you.

## What Each File Does

`index.html`

This is the main app. It contains the screens, styles, buttons, salah logging, qaza tracking, stats, settings, imports, exports, and most of the JavaScript.

`manifest.json`

This tells the browser that the tracker can behave like an installable app. It controls the app name, icon, theme colour, start page, and phone/desktop install details.

`sw.js`

This is the service worker. It caches the app files so the tracker can keep opening offline after it has successfully loaded once.

`icon.svg`

This is the app icon.

## How Your Data Is Saved

Your data is not saved inside `index.html`. It is saved in your browser storage on the same device.

The main storage key is:

`salah_tracker_v1`

That stores:

Prayer log.

Qaza totals.

Qaza repayments.

Prayer times.

Notes, khushoo, rakats, and status values.

The app also uses:

`salah_backup_reminder`

This remembers when it last reminded you to export a backup.

Safety and recovery keys may also appear:

`salah_safety_...`

`salah_safety_reason_salah_safety_...`

`salah_corrupt_backup_...`

These are there to help recover data if an import, reset, test-data load, or corrupted storage read causes trouble.

Important: if you clear browser site data, use a different browser profile, use private/incognito mode, or move to another device, the tracker may look empty until you import a backup.

## Data Safety Improvements Added

On 2026-05-06, Codex added extra data-integrity protection.

The app now:

Keeps the correct default data shape for prayer logs, qaza totals, repayments, and prayer times.

Normalizes imported data instead of blindly accepting any JSON shape.

Preserves qaza default fields for all five prayers.

Filters invalid repayment rows.

Turns invalid owed/safar numbers into safe `0` values.

Creates a safety copy before pasted JSON import.

Creates a safety copy before file import.

Creates a safety copy before prayer-times import.

Creates a safety copy before loading test data.

Creates a safety copy before reset.

Keeps a recovery copy if saved data cannot be parsed.

Shows a warning if browser storage fails while saving.

## Backups

Backups matter a lot because your real data lives in browser storage.

Use Settings -> Import / Export to export your data.

Recommended backup routine:

1. Export after setting up the tracker.
2. Export after major changes.
3. Export before importing prayer times or another data file.
4. Export before reset or test data.
5. Export regularly if you rely on the app.

Good backup names:

`salah-tracker-data-YYYY-MM-DD.json`

`salah-tracker-folder-backup-YYYY-MM-DD`

The data backup protects your salah records. The folder backup protects the app files.

## What Was Tested

On 2026-05-06, Codex ran these checks:

`sw.js` JavaScript syntax check: passed.

`manifest.json` JSON parse check: passed.

`index.html` app script syntax check: passed.

Browser load check at `http://127.0.0.1:4174/index.html`: passed.

Service worker registration: passed.

The app showed 5 prayer cards.

A prayer card expanded correctly.

Marking Fajr as `Ontime` wrote data into browser storage.

Settings -> Import / Export opened correctly.

Console/runtime errors during the browser checks: none found.

## What A JS Execution Check Means

A JS execution check means the app is actually run, not just read.

Useful levels:

1. Syntax check: catches broken JavaScript or JSON.
2. Browser load check: opens the app and checks for runtime errors.
3. Interaction check: clicks important features and confirms data is saved.

For this app, a good future check is:

1. Serve the folder through a local server.
2. Open `http://127.0.0.1:4174/index.html`.
3. Confirm the page title is `Salah Tracker`.
4. Confirm Today, History, Qaza, Stats, and Settings are visible.
5. Expand a prayer card.
6. Mark a prayer as `Ontime`.
7. Confirm data appears under `salah_tracker_v1` in browser local storage.
8. Open Settings -> Import / Export.
9. Check the browser console for red errors.

## Opening It With A Local Server

This command can serve the folder:

```powershell
python -m http.server 4174 --bind 127.0.0.1 --directory "C:\Users\yrehm\Downloads\salah-tracker"
```

Then open:

`http://127.0.0.1:4174/index.html`

If `python` is not available, Codex can use its bundled Python runtime.

## If The App Looks Empty

Try these in order:

1. Make sure you are using the same browser and browser profile.
2. Do not use private/incognito mode for your real tracker.
3. Check whether you opened the same URL or file as before.
4. Open Settings -> Import / Export and import your latest backup.
5. Ask a helper to check browser local storage for `salah_tracker_v1`.
6. Ask a helper to check for safety keys starting with `salah_safety_` or `salah_corrupt_backup_`.

Do not immediately add lots of new data if the app unexpectedly looks empty. First look for backups or safety copies.

## If Offline Mode Stops Working

Offline mode depends on `sw.js`.

The current service worker cache name is:

`salah-v2`

Try:

1. Open the app through the local server while online.
2. Refresh it.
3. Close and reopen the browser.
4. If old files keep showing, ask a helper to clear the service worker/cache or update the cache name in `sw.js`.

## If Install Does Not Work

Install/app mode depends on:

`manifest.json`

`icon.svg`

`sw.js`

A proper URL such as `http://127.0.0.1:4174/index.html` or a hosted `https://` website.

It may not install correctly from a direct `file://` open.

## If Someone Needs To Recreate Or Repair The App

Give them this folder and tell them:

The app is a local-first single-page PWA called Salah Tracker.

Most of the app is in `index.html`.

Data is saved in browser `localStorage` under `salah_tracker_v1`.

The app has an offline service worker in `sw.js`.

The app has install settings in `manifest.json`.

It should be tested through a local server, not only by double-clicking the file.

The recreated app should preserve:

Today salah logging.

Prayer statuses: ontime, late, qaza, missed.

Rakat tracking.

Khushoo tracking.

Notes.

Prayer times import.

History calendar.

Qaza totals and repayments.

Stats.

Import/export backup tools.

Safety copies before risky data changes.

Offline/PWA support.

## Suggested Prompt For Future Codex Help

AI helpers have different abilities. Some can directly read files on your computer. Many cannot. If the AI cannot access files, paste this README, describe what you see, and upload or paste the relevant file contents when asked.

Use this version if the AI can access your files:

```text
I have a local Salah Tracker PWA in:
C:\Users\yrehm\Downloads\salah-tracker

Please inspect index.html, manifest.json, sw.js, and icon.svg. Do not delete or overwrite my real data. My salah data is stored in browser localStorage under salah_tracker_v1. Start a local server for the folder, open http://127.0.0.1:4174/index.html, run syntax and browser execution checks, check for console errors, confirm the service worker registers, test expanding a prayer card, mark one prayer in a throwaway browser session, confirm localStorage saves, and check Settings -> Import / Export. If you make changes, keep data integrity and backups safe, and explain everything in plain English.
```

Use this version if the AI cannot access your files:

```text
I have a local Salah Tracker PWA, but you probably cannot access my computer files directly. Please help me safely troubleshoot or recreate it from the information I paste.

Important facts:
- The app folder is C:\Users\yrehm\Downloads\salah-tracker
- The files are index.html, manifest.json, sw.js, and icon.svg
- The main app logic is in index.html
- Salah data is stored in browser localStorage under salah_tracker_v1
- Backup reminder data is stored under salah_backup_reminder
- Safety/recovery copies may start with salah_safety_ or salah_corrupt_backup_
- Offline support comes from sw.js
- Install/app mode comes from manifest.json
- Backups are essential because the real salah data is in the browser, not inside index.html

Please ask me for only the specific thing you need next. For example, ask me to paste an error message, a screenshot, the README, the manifest, the service worker, or a small section of index.html. Explain each step in plain English and do not assume you can inspect my machine.
```

## What To Paste Into An AI Chat

If an AI cannot access your files, paste information in this order:

1. What you are trying to do.
2. What went wrong, in your own words.
3. A screenshot or exact error message, if there is one.
4. This README.
5. `manifest.json`.
6. `sw.js`.
7. The relevant part of `index.html`, only if the AI asks for it.

Avoid pasting huge files immediately unless the AI says it can handle them. Ask it: "Which exact section do you need?"

## Flexible AI Support Checklist

Ask the AI to help you check:

Does the app open?

Does it show Today, History, Qaza, Stats, and Settings?

Does expanding a prayer card work?

Does marking a test prayer work?

Does Settings -> Import / Export open?

Does export/backup still work?

Does import create or preserve a safety copy before replacing data?

Does the browser console show any red errors?

Is the app using the same browser profile where your real data lives?

## Plain-English Summary

This app is a small website that lives on your computer. The files make the app work, but your actual salah history lives in your browser storage. Keep exports of your data, keep a copy of the folder, and run a browser check after any code changes. If an AI helper cannot see your files, use this README as the map and paste only the specific files or errors it asks for.
