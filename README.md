# The Stonehouse Company — Internal Dashboard

The shared internal workspace for The Stonehouse Company. One page, four tabs — one per division. Built to be opened by anyone on the team and grown out tab-by-tab as we systematize each division.

## What's in here

```
Dashboard/
├── index.html                   ← the dashboard (open in any browser)
├── README.md                    ← this file
├── .gitignore
├── assets/                      ← drop logos, photos, exports here
└── docs/
    ├── apps-script-doGet.md     ← (legacy, kept for later) Sheet-feed code
    └── github-setup.md          ← one-time GitHub push setup
```

## Tabs

| # | Tab                                  | Status                            |
|---|--------------------------------------|-----------------------------------|
| 01 | Sales                               | Placeholder — content coming      |
| 02 | Land Acquisition                    | Placeholder — content coming      |
| 03 | Custom Home Owners Representation   | Placeholder — content coming      |
| 04 | Development                         | Placeholder — content coming      |

Each tab is its own deep-linkable view: append `#sales`, `#land`, `#customrep`, or `#development` to the URL to land directly on that tab.

Keyboard: with focus on the tab strip, use **← / → / Home / End** to move between tabs.

## Running it

Open `index.html` directly in any browser (double-click on Mac). No build step, no server, no dependencies.

## Brand tokens

| Token            | Value      | Origin                                 |
|------------------|------------|----------------------------------------|
| Background       | `#000000`  | Site `color_15`                        |
| Text             | `#FFFFFF`  | Site `color_36`                        |
| Muted            | `#B4B4B4`  | Site `color_13`                        |
| Dim              | `#707070`  | Site `color_14`                        |
| Display font     | Aboreto    | Site's "luxury" display face           |
| Body font        | Montserrat | Stand-in for Brandon Grotesque (paid)  |
| Letter-spacing   | 0.25em+    | Site's signature caps tracking         |

## Roadmap

Build out each tab one at a time. Likely order of buildout:

1. **Sales** — first division to instrument; live YTD numbers via the Sales Commission Tracker (`docs/apps-script-doGet.md` has the Apps Script code from the previous design).
2. **Development** — second priority; the new spec-home arm.
3. **Land Acquisition** — pipeline view across active sourcing efforts.
4. **Custom Home Owners Representation** — active client roster and project stage tracking.

Once tabs have real content, decide whether to host this on a real URL (Cloudflare Pages / Netlify) so the team can access it without cloning the repo.

## Repository

This lives in `The-Stonehouse-Company/Dashboard` on GitHub (private). See `docs/github-setup.md` for the one-time setup; day-to-day, just Commit and Push in GitHub Desktop.
