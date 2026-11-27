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

## Part Lifespan

OUT / RETURN / Request OUT have an optional **Machine / Location used** field (stored as `machine`).
**Reports → Part Lifespan** shows, per part + machine, the days between OUTs (average / min / max), estimated use per year,
last OUT, **Next change** (overdue / within 14 days) and "Failed early". RETURNs cancel the latest
earlier OUT, same-day OUTs count once, stocktake adjustments are ignored. Export Excel included.

## PM Schedule

**PM** in the menu: PM date = last OUT on that machine + 85% (adjustable) of the part's usual life,
for part + machine pairs with 3+ OUTs and a steady life. Stock check, "Done → record OUT", and
**Export for VELOXA** (an .xlsx for VELOXA's Import Excel; re-importing updates the same tasks).

## Spare Part Budget

**Reports → Spare Part Budget**: next 12 months per part = estimated use per year × Unit Cost, with
contingency (default +10%), "to buy" after stock on hand, breakdown by category / supplier / location,
and Excel export. Parts without a Unit Cost are flagged.

## Disable a user (staff who left)

Users → **Disable** on the person (not on yourself). Stored as `users/{uid}.status = 'disabled'`.
They are signed out at once if the app is open and cannot sign in until **Enable**. Their history stays.
For a full lock-out also disable the login in Firebase Console → Authentication → Users → ⋮ → Disable account
(the app check runs in the browser; Firestore rules decide what a signed-in account can reach).
Test (emulators): `npm run e2e:v1-users` in sharulapps/sparetrak.

## Stock Valuation

KPI card **Stock Value** (stock × Unit Cost, change vs 30 days ago) and **Reports → Stock Valuation**:
total, dead stock (no OUT for 6/12/24 months), value above Max Stock, breakdown by category / location /
supplier, Excel export.
