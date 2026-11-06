# SpareTrack

Spare part management PWA (single-file `index.html` + Firebase), hosted on Cloudflare Pages.

## Layout

```
public/              ← Cloudflare Pages output dir (everything here is public)
  index.html         the app
  manifest.json      PWA manifest
  sw.js              service worker (cache: sparetrack-v2)
  _headers           Cloudflare Pages headers
  icon-192.png, icon-512.png
  tutorials/         Help videos (*.mp4, *.jpg posters, tutorials.json) — demo data only
tools/
  setup-tool.html    config patcher for new customers — NOT deployed, open locally
```

## Deploy (Cloudflare Pages)

- Framework preset: None
- Build command: (empty)
- Build output directory: `public`

When you change cached files, bump `CACHE` in `public/sw.js` so clients pick up the new version.

## Help videos

The **Help** menu (and "How to use SpareTrack" on the sign-in screen) plays the videos in
`public/tutorials/`. They are recorded on Firebase emulators with a demo company (ABC Sdn Bhd),
never live data, by `tools/record-tutorials.mjs` in the sharulapps/sparetrak repo:

```bash
# in sparetrak, with this repo checked out next to it
TUT_APP=../sparetrack/public TUT_OUT=../sparetrack/public/tutorials npm run tutorials:v1
```
