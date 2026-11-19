# Where to edit things (plain-English guide)

You can change any of these yourself on the GitHub website: open the file,
click the **pencil icon** (Edit), make your change, then click
**Commit changes**. The live app updates a few minutes later.
Or just ask Claude to do it for you.

Search tip: inside most files, look for the words **EDIT ME**.

| I want to change… | File | What to look for |
|---|---|---|
| The app's name (top of the screen) | `index.html` | `Ashdown Survey` (appears twice) |
| The name under the home-screen icon | `manifest.webmanifest` | `"name"` and `"short_name"` |
| The questions on the record form | `index.html` | `EDIT ME: Example question 1` and `EDIT ME: Example question 2` |
| The answers in the pick-one list | `index.html` **and** `js/app.js` **and** `firestore.rules` | `Answer A`… / `option_a`… (all three must match) |
| The error messages if something is missing | `js/app.js` | `Please answer example question` |
| The help page volunteers read (Info tab) | `js/info.js` | anything in `[[ double brackets ]]` |
| Colours | `css/app.css` | the list of colours at the very top |
| The logo | `icons/logo.svg` | replace with your own square image |
| Your survey site outline (for offline maps) | `data/fires-index.geojson` | ask Claude to make this from a map or a place name |
| Map key and shared database settings | `js/config.js` | `YOUR_MAPTILER_KEY` and `FIREBASE` |

## Two things to remember

1. **After changing anything in a `.js` file**, open `sw.js` and add 1 to the
   number in `VERSION` (for example `v30` → `v31`). Otherwise phones keep the old
   version. Phones pick up the new version the second time the app is opened.
2. **`firestore.rules` only takes effect when you paste it into the Firebase
   website** (Firestore → Rules → Publish). Changing it on GitHub alone does nothing.

## Words you will see in the code

The original app recorded wildfire damage, so the code still uses some fire
words behind the scenes. You can ignore them. Volunteers never see them.

- "fire" = your survey site
- "depth" / "depths" = example question 1 (the numbers)
- "combustion" = example question 2 (the pick-one list)
- "measurement" / "point" = one record
