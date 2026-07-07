# Property Management — Setup

The Property Management division has 6 subtabs — Overview, **Properties**, **Tenants**, **Work Orders**, Owner Reports (phase 2), and Files. The three data-driven subtabs each need their own Google Sheet + Google Form (same pattern as Clients, Buyer Deals, etc.).

Plan ~30 minutes total: 3 Sheets, 3 Forms, 3 sets of entry IDs to paste.

---

## 1 · Create the three Sheets

Log into Google as `thestonehousecompanyadmin@gmail.com`.

For each, go to [sheets.new](https://sheets.new) and rename:

| Sheet name | Purpose |
|---|---|
| **Stonehouse Properties** | Every property under management |
| **Stonehouse Tenants** | Tenant contacts + lease info |
| **Stonehouse Work Orders** | Maintenance requests |

Share → Anyone with the link → Viewer for each.

---

## 2 · Create the three Forms

### 2a · Properties form

Name it **Add Property**. Add these questions IN ORDER with these exact lowercase labels:

| # | Label | Type |
|---|---|---|
| 1 | property_address | Short answer · required |
| 2 | property_type | Dropdown: Single Family / Condo / Multi-Family / Townhome / Other |
| 3 | unit_count | Short answer (number) |
| 4 | bedrooms | Short answer (number) |
| 5 | bathrooms | Short answer (number) |
| 6 | neighborhood | Short answer |
| 7 | owner_name | Short answer · required |
| 8 | owner_email | Short answer |
| 9 | owner_phone | Short answer |
| 10 | owner_mailing | Short answer |
| 11 | monthly_rent | Short answer (number) |
| 12 | mgmt_fee_pct | Short answer (number) |
| 13 | lease_start | Date |
| 14 | lease_end | Date |
| 15 | occupied | Multiple choice: Yes / No |
| 16 | tenant_name | Short answer |
| 17 | notes | Paragraph |

Settings → Responses → Link to Sheet → **Stonehouse Properties**.

### 2b · Tenants form

Name it **Add Tenant**. Fields:

| # | Label | Type |
|---|---|---|
| 1 | first_name | Short answer · required |
| 2 | last_name | Short answer · required |
| 3 | email | Short answer |
| 4 | phone | Short answer |
| 5 | emergency_contact | Short answer |
| 6 | status | Dropdown: Active / Notice Given / Late / Past |
| 7 | property_address | Short answer · required |
| 8 | unit | Short answer |
| 9 | monthly_rent | Short answer (number) |
| 10 | deposit | Short answer (number) |
| 11 | lease_start | Date · required |
| 12 | lease_end | Date · required |
| 13 | move_in | Date |
| 14 | pet_fees | Short answer |
| 15 | notes | Paragraph |

Link to **Stonehouse Tenants** sheet.

### 2c · Work Orders form

Name it **Log Work Order**. Fields:

| # | Label | Type |
|---|---|---|
| 1 | property_address | Short answer · required |
| 2 | unit | Short answer |
| 3 | tenant_name | Short answer |
| 4 | category | Dropdown: Plumbing / Electrical / HVAC / Appliance / Roof / Landscaping / Pest Control / General Repair / Other · required |
| 5 | priority | Dropdown: Emergency / High / Medium / Low · required |
| 6 | description | Paragraph · required |
| 7 | reported_date | Date |
| 8 | scheduled_date | Date |
| 9 | vendor | Short answer |
| 10 | vendor_phone | Short answer |
| 11 | estimated_cost | Short answer (number) |
| 12 | actual_cost | Short answer (number) |
| 13 | status | Dropdown: Reported / Scheduled / In Progress / Complete / Cancelled · required |
| 14 | completed_date | Date |
| 15 | notes | Paragraph |

Link to **Stonehouse Work Orders** sheet.

---

## 3 · Get the formResponse URLs and entry IDs

For each form:

1. Send → Link → copy the viewform URL → replace `/viewform` with `/formResponse`. That's the `FORM_URL`.
2. Form → ⋮ menu → **Get pre-filled link** → fill placeholder values → copy the resulting link.
3. Extract every `entry.NNN` value.

---

## 4 · Wire the CONFIGs in `index.html`

Find these three blocks near the "PROPERTY MANAGEMENT" section header and fill in:

```js
const PROPERTIES_CONFIG = {
  SHEET_ID:  "…paste the Properties sheet ID…",
  SHEET_TAB: "Form Responses 1",
  FORM_URL:  "https://docs.google.com/forms/d/e/…/formResponse",
  FIELD_MAP: {
    property_address: "entry.…",
    property_type:    "entry.…",
    // ...all 17 entry IDs...
  }
};

const TENANTS_CONFIG = { /* same shape, 15 entry IDs */ };
const WORKORDERS_CONFIG = { /* same shape, 15 entry IDs */ };
```

The order of fields in `FIELD_MAP` matches the modal HTML in the Add Property / Add Tenant / Log Work Order modals.

---

## 5 · Commit + push

Once all three CONFIGs are filled in, commit and push. GitHub Pages picks up within a minute.

---

## How it behaves after wiring

- **Overview** → auto-calculates all 8 tiles from the three sheets
- **Properties** → list with search + filter (occupied/type) + CSV export
- **Tenants** → list with search + status filter + CSV export
- **Work Orders** → 4-stage pipeline (Reported / Scheduled / In Progress / Complete). Emergency-priority WOs sort to top. Complete list shows last 30 days only.
- **Owner Reports** → placeholder (phase 2 — build when you have income/expense data)
- **Files** → static PDF cards (currently placeholders for the 4 templates: Management Agreement, Owner Onboarding, Tenant Application, Lease Template)

**Autocomplete**: When adding a tenant or work order, the property address field auto-suggests from the loaded Properties list. Type the first few characters and pick — keeps addresses spelled consistently across sheets.

**Cross-sheet linking**: Tenants + Work Orders reference properties by `property_address` (free text). Keep the address spelling identical to what's in the Properties sheet so tiles and filters work correctly.

---

## Editing existing rows

Forms only ADD. To update a property's rent, occupancy, or notes — edit the row directly in the Google Sheet. Same for tenants and work orders. Next dashboard load reflects it.

To close a work order: change its `status` cell in the sheet to `Complete` and set `completed_date`. Dashboard moves it from the In Progress column to the Complete column on next load.
