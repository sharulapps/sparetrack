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
tools/
  setup-tool.html    config patcher for new customers — NOT deployed, open locally
```

## Deploy (Cloudflare Pages)

- Framework preset: None
- Build command: (empty)
- Build output directory: `public`

When you change cached files, bump `CACHE` in `public/sw.js` so clients pick up the new version.
