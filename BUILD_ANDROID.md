# Building MeasureUp for Android (Capacitor)

This project is already scaffolded — `package.json` and `capacitor.config.json`
are in place, and the web app lives in `www/`. You just need to install
dependencies and add the native Android platform.

## Prerequisites (one-time, outside this project)
- [Node.js](https://nodejs.org) installed (`node -v` to check)
- [Android Studio](https://developer.android.com/studio) installed, with the
  Android SDK set up (Android Studio prompts you through this on first launch)

## Setup — run these from the project root, in order

```bash
# 1. Install the Capacitor packages listed in package.json
npm install

# 2. Add the Android native project (creates an `android/` folder here)
npx cap add android

# 3. Copy www/ into the native project and sync config
npx cap sync android
```

At this point you'll have a full native Android Studio project in `android/`.

## Generate proper Android icon assets (recommended)

Right now the app uses one placeholder icon. Capacitor has a tool that takes
a single source image and generates every Android icon size/format for you
(adaptive icons, splash screen, etc.):

```bash
# Put a 1024x1024 PNG of your final icon at: resources/icon.png
mkdir -p resources
cp www/icons/icon-512.png resources/icon.png   # swap this for your real icon later

npx capacitor-assets generate --android
```

Re-run this any time you get the final app icon from your source.

## Open, run, and build

```bash
# Opens the project in Android Studio
npx cap open android
```

From Android Studio:
- Click the green **Run ▶** button to install on a connected phone or emulator
- **Build → Generate Signed Bundle / APK** to produce a real installable
  `.apk` (for sharing/sideloading) or `.aab` (for uploading to the Play Store)

## After any change to the web app

Whenever you edit files in `www/` (index.html, manifest.json, sw.js, icons),
re-sync before rebuilding:

```bash
npx cap sync android
```

## Before publishing to the Play Store

- Change `appId` in `capacitor.config.json` from `com.measureup.app` to your
  own unique reversed-domain ID — this can't be changed after you publish.
- You'll need a one-time $25 Google Play developer account.
- The signed `.aab` from the step above is what you upload to the Play
  Console.

## Notes specific to this app

- The app already works fully offline once installed — its service worker
  (`www/sw.js`) caches the app shell, and data is stored locally per-device
  via `localStorage`, so no backend or internet connection is required.
- `capacitor.config.json` sets the Android status bar / splash background to
  `#141922` to match the app's dark theme, so there's no white flash on launch.
