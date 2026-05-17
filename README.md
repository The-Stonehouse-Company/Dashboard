# The Stonehouse Company — Dashboard

A single-file, brand-matched public hub for The Stonehouse Company. Built to mirror the visual identity of [thestonehouseco.com](https://thestonehouseco.com) — black background, Aboreto display type, wide-tracked all-caps — with a live "Tape" of Sales Division metrics pulled from the Sales Commission Tracker Google Sheet.

## What's in here

```
Dashboard/
├── index.html                   ← the dashboard (open in any browser)
├── README.md                    ← this file
├── .gitignore
├── assets/                      ← drop logos, photos, project images here
└── docs/
    ├── apps-script-doGet.md     ← code to add to the Sheet's Apps Script
    └── github-setup.md          ← step-by-step to push this to GitHub
```

## Running it locally

1. **Open `index.html`** directly in any browser (double-click it on Mac).
2. To wire up the live Sales metrics, follow `docs/apps-script-doGet.md` — paste the resulting `/exec` URL into the `SHEETS_API_URL` constant near the bottom of `index.html`.
3. Until you do step 2, the four metric tiles will show `—` and "Live feed not configured" in the corner. Everything else renders fine.

## Sections

| Section            | Source                                          |
|--------------------|--------------------------------------------------|
| Hero               | Static — pulled from brand                       |
| The Tape (metrics) | **Live** from Sales Commission Tracker via Apps Script |
| Four Divisions     | Static — pulled from `Company Overview.docx`     |
| Platinum Reserve   | Static — sub-brand callout                       |
| Featured Projects  | Static placeholders (Davis, Arrington, Clifton)  |
| Geography          | Static — Middle TN                               |
| Mission & Values   | Static                                           |
| Footer / Contact   | Static                                           |

## Brand tokens used

| Token            | Value      | Origin                                 |
|------------------|------------|----------------------------------------|
| Background       | `#000000`  | Site `color_15`                        |
| Text             | `#FFFFFF`  | Site `color_36`                        |
| Muted            | `#B4B4B4`  | Site `color_13`                        |
| Dim              | `#707070`  | Site `color_14`                        |
| Display font     | Aboreto    | Site's "luxury" display face           |
| Body font        | Montserrat | Stand-in for Brandon Grotesque (paid)  |
| Letter-spacing   | 0.25em+    | Site's signature caps tracking         |

## Roadmap (later)

- Replace the SVG map with a proper geo / interactive map of active listings.
- Pull "Featured Projects" live from a `Projects` sheet tab once one exists.
- Add a private internal-ops view (separate page) tied to the Land Acquisition pipeline and Development underwriting model.
- Move from local-file usage to a hosted private URL when the company is ready to share access.

## Repository

This lives in the `The-Stonehouse-Company` GitHub organization. See `docs/github-setup.md` for the one-time push instructions.
