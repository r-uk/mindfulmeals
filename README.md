# Mindful Meals

A single-file, offline-first app for slowing down at meals: photograph the
plate, run a timer while you eat, then rate portion size, when you stopped,
and how well you chewed. Everything is tracked locally to build a streak.

**No account, no server, no network calls.** The whole app is one HTML file.
Your meal photos, ratings, and notes are stored only in your browser's local
storage on your own device and are never uploaded anywhere.

---

## What's in this repo

| File | Needed for |
|---|---|
| `index.html` | The app itself. This is the only file required to use it at all. |
| `manifest.json` | Lets the app be "installed" with a home-screen icon and no browser bar. |
| `sw.js` | Service worker — caches the app so it opens with no internet connection. |
| `icon-192.png`, `icon-512.png` | App icons used by the manifest / installed app / APK. |

If you only want to open the app and use it, you only need `index.html`.
The other files are only needed for installing it as an app or building the APK.

---

## Option 1 — Just open the file (fastest, zero setup)

1. Download `index.html`.
2. Double-click it, or drag it into any browser window.

That's it. It runs entirely offline, works the same on desktop or mobile, and
nothing is installed. Your data stays wherever your browser stores local site
data for that file — if you move or rename the file's location significantly
(or clear browser data), you'd lose history, so export a backup from
**Settings → Export JSON** occasionally.

---

## Option 2 — Use it in your phone's browser, installed like an app

This uses the hosted version (e.g. GitHub Pages), not the raw file, so the
manifest and service worker can register.

### Android (Chrome)
1. Open the hosted URL in Chrome.
2. Tap the **⋮** menu → **Add to Home screen** / **Install app**.
3. It now opens full-screen from a home-screen icon, no address bar, and
   keeps working offline after the first load.

### iOS (Safari)
1. Open the hosted URL in Safari (must be Safari — other iOS browsers can't
   install web apps).
2. Tap the **Share** icon → **Add to Home Screen**.
3. It opens full-screen from the home screen. Safari doesn't use the service
   worker the same way Chrome does, so keep a connection for the very first
   load after any update.

---

## Option 3 — Install as a real Android app (APK), including GrapheneOS / sandboxed setups

This is a plain Android APK — it contains no Google Play services calls, no
analytics SDK, and requests no network permission it doesn't need (it makes
no network calls at all once installed). That makes it a good fit for
de-Googled or sandboxed setups like GrapheneOS, /e/OS, or a stock phone with
a work profile / isolated user you don't want online.

1. Get `mindful-meals.apk` (built via PWABuilder from the hosted site — see
   repo history / releases, or build it yourself by pointing
   [pwabuilder.com](https://www.pwabuilder.com) at the hosted URL and using
   its **Google Play** packaging option, which outputs a signed/unsigned
   `.apk`).
2. Transfer the `.apk` to the device (cable, cloud file, or download
   directly if the device has any browser access).
3. Open the file. Android will prompt to allow installs from that source —
   approve just for this install if you want to keep the "unknown sources"
   permission narrow.
4. Install. No Play Store account, no Google services, and no network
   connection are required for the app to run — you can leave the device
   offline permanently and it will work exactly the same.
5. Optional hardening for sandboxed installs: deny the app any network
   permission at the OS level (e.g. GrapheneOS's per-app internet toggle,
   or a firewall app) — the app never needs it, so blocking it costs you
   nothing and guarantees no data ever leaves the device even if a future
   version tried to.

---

## Your data

- Stored in IndexedDB (falls back to localStorage, then in-memory) on the
  device/browser you're using — not synced, not backed up automatically.
- **Settings → Export JSON** gives you a full backup file; **Import a
  backup** restores it. Only ever import a backup file you created yourself.
- **Settings → Export CSV** gives a spreadsheet-friendly view without photos.
- **Settings → Delete everything** wipes all local data permanently.

## Notes on privacy

- Meal photos are re-encoded through a canvas before saving, which strips
  any location/EXIF metadata automatically.
- Nothing in this app phones home. If you're auditing it: search the source
  for `fetch`, `XMLHttpRequest`, or `<script src=` pointing off-device —
  there are none.
