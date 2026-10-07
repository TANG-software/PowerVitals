# PowerVitals — web version

A free, licence-free web version of the PowerVitals battery dashboard, with ad
slots ready to fill. No pricing page, no unlock code, no sign-in.

## Files

- `index.html` — the whole site (HTML + CSS + JS in one file). Self-contained.
- `README.md` — this file.

## What it does

- **Live battery dashboard** — reads level, charging state and charge/discharge
  timing straight from the browser (Chrome on Android gives the most detail).
- **Feature overview** — describes what the full Android app adds.
- **Download section** — a button to hand out the Android app.
- **Two ad slots** — marked in `index.html` as `#ad-top` and `#ad-bottom`.

Everything runs locally in the visitor's browser. Nothing is uploaded anywhere.

## Add your ads

Open `index.html` and find the two `<div class="ad" ...>` blocks. Delete the
placeholder text inside and paste your ad network's snippet (e.g. a Google
AdSense `<ins class="adsbygoogle">` block) where indicated.

## Put the Android APK next to the page (optional)

The download button points at `PowerVitals.apk`. To make it work, drop your APK
file into this same folder and name it `PowerVitals.apk`. It will then be
served as a normal download.

## Deploy — Vercel (recommended)

1. Put `index.html` (and `PowerVitals.apk`, if you have it) in a folder.
2. Go to https://vercel.com and sign in (GitHub/email).
3. Click **Add New → Project → Deploy** and either:
   - drag the folder onto the Vercel dashboard, or
   - push it to a GitHub repo and import that repo.
4. Framework preset: **Other**. Build command: leave empty. Output dir: leave empty.
5. Deploy. You get a live `https://<name>.vercel.app` URL, and you can attach a
   custom domain later.

No build step is needed — it's plain static HTML.

## Deploy — alternatives

- **Netlify** — same idea: drag the folder onto https://app.netlify.com/drop.
- **GitHub Pages** — push the folder to a repo, then Settings → Pages → deploy
  from the branch root.
- **Cloudflare Pages** — connect the repo, no build command.

Any of these works. Vercel or Netlify are the quickest.

## Important

A website cannot run an Android app. This is a **separate web version** built
from scratch — it shares the idea and branding, not the phone app's code. The
deep readings (voltage, current, temperature, calibration, cycle count) only
exist in the Android app, because browsers don't expose them.
