# PowerVitals — web view (3D)

A free, licence-free **web version** of the PowerVitals screen, with a real
**3D battery view** and a **JavaScript bridge** so the Android app can feed it
live data. On a plain browser it falls back to what browsers allow.

## Files

- `index.html` — the whole site (HTML + CSS + JS + a canvas 3D renderer). One
  file, no dependencies, works offline.
- `README.md` — this file.

## The 3D battery view

A canvas renderer draws a real 3D cuboid (projection + painter's-algorithm
depth sorting): a glass battery with a coloured liquid fill that tracks charge
level, and a terminal nub on top. It **auto-rotates** and you can **drag to
rotate** it. Colour turns amber under 35% and red under 15%.

No libraries — pure canvas, so it works inside an offline WebView.

## How the functions go live (JS bridge)

A browser alone **cannot** read voltage, current, temperature, cycle count or
sysfs, and cannot toggle Wi-Fi/Bluetooth. Those only work inside the app, where
Android hands the data to the page through a bridge.

The page looks for `window.PowerVitalsBridge` (or `window.Android`) and calls:

### `getSnapshot()` → JSON string (polled every 1s)

```json
{
  "level": 62,
  "charging": true,
  "voltageV": 4.12,
  "currentA": 0.85,
  "tempC": 31.4,
  "healthPct": 94,
  "healthRating": "Excellent",
  "designCapacityUah": 4500000,
  "actualCapacityUah": 4230000,
  "cycleCount": 210,
  "technology": "Li-ion",
  "chargeRate": 12.5,
  "timeToFull": 5400,
  "chargeAdded": "1.2 Ah | 4.6 Wh | 1200 mAh",
  "rawChargerW": 8.4,
  "intakeW": 6.1,
  "systemOverheadW": 2.3,
  "efficiencyPct": 72.6,
  "wifi": "ON",
  "bluetooth": "OFF",
  "sync": "ON",
  "chargingSource": "USB",
  "plugType": "AC",
  "chargeCounterUah": 2100000,
  "energyCounterUwh": 8800000,
  "sysfs": "voltage_now=4120000 ..."
}
```

Any field you omit is simply left showing `—`.

### Action methods (each returns a short status string)

`startCalibration()`, `clearCalibration()`, `toggleWifi()`,
`toggleBluetooth()`, `toggleSync()`, `openLocation()`, `openDisplay()`,
`optimizeAll()`

Buttons call these automatically when the bridge exists; otherwise they're
disabled.

## Wiring it in the Android (WebView) app

```java
WebSettings s = webView.getSettings();
s.setJavaScriptEnabled(true);          // REQUIRED for the 3D view + live data

webView.addJavascriptInterface(new Object() {
  @JavascriptInterface public String getSnapshot() { return buildJson(); }   // return the JSON above
  @JavascriptInterface public String toggleWifi() { /* toggle, then */ return "WiFi: ON"; }
  @JavascriptInterface public String toggleBluetooth() { return "Bluetooth: ON"; }
  @JavascriptInterface public String toggleSync() { return "Auto-Sync: ON"; }
  @JavascriptInterface public String startCalibration() { return "Calibration started"; }
  @JavascriptInterface public String clearCalibration() { return "Data cleared"; }
  @JavascriptInterface public String openLocation() { return ""; }
  @JavascriptInterface public String openDisplay() { return ""; }
  @JavascriptInterface public String optimizeAll() { return "Optimised"; }
}, "PowerVitalsBridge");

// bundle the page for offline use:
webView.loadUrl("file:///android_asset/index.html");
```

Build `buildJson()` on the Android side from the same APIs the original app
used (BatteryManager, `/sys/battery`, etc.). `@JavascriptInterface` needs API 17+.

## Get-app button

The sticky bar's **Get App** button points at `PowerVitals.apk`. Change the
`href` on `<a id="getApp" ...>` to your download link.

## Ads

Two slots: `<div class="ad" id="ad-1">` and `#ad-2`. Paste your ad code inside.
Note: many ad networks (AdSense included) restrict ads inside app WebViews.

## Deploy

Already live on GitHub Pages:
**https://tang-software.github.io/PowerVitals/**

Plain static HTML, no build step. For any other host: publish directory `.`,
build command empty, framework preset "Other".

## Important

A website cannot run the Android app or read the deep sensors. Everything past
level/charging/timing comes from the Android bridge — so those functions only
light up inside your APK.
