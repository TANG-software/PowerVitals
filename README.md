# PowerVitals — web preview

A free, licence-free **web version** of the PowerVitals screen. It mirrors the
Android app's layout (dark "diagnostic unit" theme, orange accents, monospace)
so it can be loaded inside a WebView app, with ad slots ready to fill.

## Files

- `index.html` — the whole site (HTML + CSS + JS in one file). Self-contained,
  mobile-first, works offline.
- `README.md` — this file.

## What it shows

The page copies the app's sections one-for-one:

Battery Health · Calibration · Health Snapshot · Charging Dashboard ·
Power Flow Analysis · Live Monitor · Power Consumption · Battery Optimizer ·
Advanced Data · Raw Sysfs Data

**Live in the browser:** battery level, charging state, time-to-full, and an
estimated charge rate (%/hr).

**Shown as placeholders (—):** voltage, current, temperature, capacity,
calibration, cycle count, sysfs. Browsers cannot read these — they are Android
APIs only. That is a platform limit, not a bug.

## Get-app button

The sticky bar at the bottom has a **Get App** button pointing at
`PowerVitals.apk`. Change the `href` on the `<a id="getApp" ...>` element to
whatever download link you want (your hosted APK, a Play Store URL, etc.).

## Add your ads

Find the two `<div class="ad" ...>` blocks (`#ad-1` and `#ad-2`). Delete the
placeholder text and paste your ad network's snippet where indicated.

Note: many ad networks (including Google AdSense) restrict ads inside app
WebViews. Check your network's policy before relying on in-app web ads.

## Using it inside the new APK (WebView)

If the new APK is a WebView wrapper, point it at this page:

```java
webView.getSettings().setJavaScriptEnabled(true);   // needed for the live battery JS
webView.loadUrl("file:///android_asset/index.html"); // if bundled in the APK assets
// or loadUrl("https://your-site.vercel.app") if hosted
```

Bundling `index.html` into the APK's `assets/` folder makes the app work fully
offline. The browser Battery API only returns real values on Chrome-based
WebViews.

## Deploy — Vercel (or Netlify / GitHub Pages)

Plain static HTML, no build step:

1. **Vercel** — vercel.com → Add New → Project → import this repo → Framework
   preset "Other" → Deploy.
2. **Netlify** — drag this folder onto app.netlify.com/drop.
3. **GitHub Pages** — Settings → Pages → deploy from branch root.

## Important

A website cannot run the Android app, and cannot read the deep battery sensors.
This is a visual web rebuild that shares the app's design, not its native
capabilities.
