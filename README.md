# Field-survey app template

A complete, working skeleton for an **offline-first field-survey web app** —
installable on any phone, usable with no signal, hosted for free on GitHub
Pages. It is the generalised skeleton of
[PeatProbe](https://github.com/johannaschoenecker/peatprobe), a live
citizen-science app for recording peat burn depth after UK wildfires.

**Use this template** (green button above) to create your own copy, then read
**[GETTING-STARTED.md](GETTING-STARTED.md)** — it walks through hosting, map
tiles, optional cloud sync, and making the app your own.

What's inside:

- Installable offline PWA — no build step, no npm; plain ES modules + Leaflet
- Map with toggleable layers and downloadable offline "field packs"
- Recording form: GPS (or tap-to-place), repeat readings with auto-mean,
  controlled vocabulary, required photos, comments
- Per-area 100 m sampling grid with a compass navigation arrow
- Optional Firebase sync: Google sign-in, shared points, admin review queue,
  one-tap backup, CSV export
- AR tape-free 1 × 1 m quadrat via the phone camera

Everything account-specific ships blank: add your own (free) MapTiler key and,
if you want shared data, your own Firebase project. Until then the app runs
entirely on-device.

Built by Johanna Schönecker for UK peatland fire research; template-ified so
other field teams can start from working code instead of a blank page.
