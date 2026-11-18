# Field-survey app template

This is the skeleton of **PeatProbe** (Johanna Schönecker's citizen-science app
for recording peat burn depth after UK wildfires), shared as a starting point
for your own field-survey app. The peat-specific *data* has been removed and
all account keys blanked; the *code* is complete and working.

What you get out of the box:

- An installable, offline-first web app (add to home screen on iPhone/Android)
- A Leaflet map with toggleable layers and downloadable offline "field packs"
  (base-map tiles cached per survey area, stored in the browser's IndexedDB)
- A recording form: GPS capture (or tap-to-place), up to 5 numeric readings
  with auto-mean, a controlled-vocabulary dropdown, required photo(s), comments
- A per-area 100 m sampling grid with navigate-to-point arrow (phone compass)
- Optional cloud sync (Firebase): Google sign-in, shared points, photo upload,
  an in-app admin review queue with approve/reject, backup export, CSV export
- An AR "1 × 1 m quadrat" overlay via the phone camera (bonus feature)

No build step, no framework — plain HTML/JS/CSS plus a vendored copy of
Leaflet. If you can edit a text file and push to GitHub, you can run this.

---

## 1. Run it locally (2 minutes)

Any static file server works. With Python installed:

    cd this-folder
    python -m http.server 8000

Open http://localhost:8000. The map will be blank until step 3 (no map key
yet) and there are no survey areas yet — that's expected. The service worker
is deliberately disabled on localhost so you always see your latest edits.

## 2. Put it on GitHub Pages (your hosting, free)

1. Create a repository on your GitHub account (public), e.g. `ashdown-survey`.
2. Push this folder's contents to its `main` branch (the `.nojekyll` file
   must come along — it already exists here).
3. Repo → Settings → Pages → "Deploy from a branch" → `main`, root.
4. Your app is live at `https://YOUR-USERNAME.github.io/ashdown-survey/`
   a couple of minutes after every push. Push = deploy; that's the whole
   pipeline.

**Service-worker ritual:** whenever you change any JS, bump `VERSION` in
`sw.js` (v27 → v28 …), or phones will keep running the old code. Installed
phones apply an update on the **second** launch after a deploy.

## 3. Map tiles (MapTiler key)

1. Free account at maptiler.com → Keys → create a key.
2. Paste it into `js/config.js` where it says `YOUR_MAPTILER_KEY`.
3. **Immediately** set the key's "Allowed HTTP origins" to
   `localhost` and `YOUR-USERNAME.github.io` (details in the long comment in
   `config.js`). An unrestricted key in a public repo can be spent by anyone.
4. Mind the quota: downloading an offline pack fetches every tile in the
   area in one burst. The free tier is fine for development and small areas;
   read the QUOTA comment in `config.js` before inviting many users.

## 4. Cloud sync (Firebase — optional, do it last)

The app is fully usable without this (points stay on each device; CSV export
still works). When you want shared data:

1. console.firebase.google.com → create a project.
2. **Firestore Database** → create (choose `europe-west2` for the UK) →
   paste the contents of `firestore.rules` into Rules → Publish.
3. **Authentication** → sign-in method → enable Google.
4. **Storage** → create → paste `storage.rules` into its Rules → Publish.
5. Project settings → Your apps → add a *Web app* → copy the config object
   into `FIREBASE.config` in `js/config.js` and set `enabled: true`.
6. Authentication → Settings → Authorized domains → add
   `YOUR-USERNAME.github.io`.
7. Make yourself admin: sign into the app once (My data → Sync now), find
   your UID under Authentication → Users, then in Firestore create a
   collection `admins` with one document whose **ID is that UID** (content
   can be `{}`). The Review tab appears for admins only.

Notes: the config values are identifiers, not secrets — safe in a public
repo. All real access control lives in the two rules files. **Rules only take
effect when pasted and published in the console** — pushing them to GitHub
does nothing. The rules accept points inside a UK bounding box only
(lat 49–61, lon −9–2.5) — fine for Ashdown Forest; edit `firestore.rules`
and the matching check in `js/app.js` (`savePoint`) if you ever survey
elsewhere.

## 5. Make it yours

- **Name** — a search-and-replace for "PeatProbe" across five files does it:
  `js/config.js` (`APP.name`), `index.html` (page title + top bar),
  `manifest.webmanifest` (`name`, `short_name` — what the home-screen icon is
  labelled), `js/info.js` (the about text — rewrite this for your survey
  anyway), and optionally the cache name in `sw.js`.
- **Logo** — replace `icons/logo.svg` with your own (any square SVG), then,
  with the step-1 server running, open
  `http://localhost:8000/tools/make-icons.html` and click the button to
  regenerate the PNG set
  (`icon-180` is what iPhones show on the home screen; 192/512 are Android).
  The top-bar logo and favicon pick up the SVG automatically.
- **Colours**: the palette is ~10 CSS variables at the top of `css/app.css`.
- **Survey areas** (PeatProbe calls them "fires" throughout the code):
  replace `data/fires-index.geojson` with a FeatureCollection of your survey
  area polygons (WGS84). Each feature needs `properties.id` (unique string);
  names shown in the UI come from `fireName()` in `js/packs.js` — adapt it to
  your properties. Detailed per-area outlines go in `data/fires/<id>.geojson`.
- **Sampling grids**: `tools/build_sampling_grid.py` generates the 100 m
  OS-grid nodes per area (`data/grid/<id>.json`). It needs Python with GDAL
  (QGIS's bundled Python works — see the notes at the top of the script).
- **Form fields**: the reading labels and the dropdown vocabulary live in
  `index.html` + `COMBUSTION` in `js/app.js`; keep the Firestore rules'
  vocabulary check in sync.
- **Peat-fire extras you may not need**: the burn-severity (dNBR) overlay and
  the land-cover chart (`js/chart.js`, `data/dnbr/`) are wildfire-specific.
  They stay dormant if you never generate that data — safe to ignore or
  delete. Same for `tools/gee_*.js` (Google Earth Engine exports).

## 6. Module map (where to look when changing things)

    index.html   screens: map, record form, packs, my-data, review, info
    js/app.js    boot, form logic, validation, tabs, CSV export, backup
    js/map.js    map, layers, offline tiles, area polygons, grid, popups
    js/packs.js  offline field-pack download/estimate
    js/db.js     IndexedDB (points, photos, tiles, packs)
    js/sync.js   Firebase: auth, push/pull, review queue, admin helpers
    js/review.js admin review cards
    js/nav.js    walk-to-point navigation arrow
    js/grid.js   sampling-grid loading & nearest-node maths
    js/ar.js     AR quadrat
    js/info.js   info/about tab content  ← update the protocol text!
    sw.js        service worker (offline shell; bump VERSION on JS changes)

`FIREBASE-SETUP.md` is the original step-by-step Firebase guide with more
detail (in German — the console steps match section 4 above).

Good luck — and credit/questions: the original app is
https://github.com/johannaschoenecker/peatprobe.
