# Property Management — Setup

The Property Management division has 5 subtabs — Overview, **Properties**, **Tenants**, Owner Reports (phase 2), and Files. The two data-driven subtabs each need their own Google Sheet + Google Form (same pattern as Clients, Buyer Deals, etc.).

Plan ~20 minutes total: 2 Sheets, 2 Forms, 2 sets of entry IDs to paste.

---

## 1 · Create the two Sheets

Log into Google as `thestonehousecompanyadmin@gmail.com`.

For each, go to [sheets.new](https://sheets.new) and rename:

| Sheet name | Purpose |
|---|---|
| **Stonehouse Properties** | Every property under management |
| **Stonehouse Tenants** | Tenant contacts + lease info |

Share → Anyone with the link → Viewer for each.

---

## 2 · Create the two Forms

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

---

## 3 · Get the formResponse URLs and entry IDs

For each form:

1. Send → Link → copy the viewform URL → replace `/viewform` with `/formResponse`. That's the `FORM_URL`.
2. Form → ⋮ menu → **Get pre-filled link** → fill placeholder values → copy the resulting link.
3. Extract every `entry.NNN` value.

---

## 4 · Wire the CONFIGs in `index.html`

Find these two blocks near the "PROPERTY MANAGEMENT" section header and fill in:

```js
const PROPERTIES_CONFIG = { SHEET_ID: "…", SHEET_TAB: "Form Responses 1", FORM_URL: "…", FIELD_MAP: { /* 17 entry IDs */ } };
const TENANTS_CONFIG    = { SHEET_ID: "…", SHEET_TAB: "Form Responses 1", FORM_URL: "…", FIELD_MAP: { /* 15 entry IDs */ } };
```

The order of fields in `FIELD_MAP` matches the modal HTML in the Add Property / Add Tenant modals.

---

## 5 · Commit + push

Once both CONFIGs are filled in, commit and push. GitHub Pages picks up within a minute.

---

## How it behaves after wiring

- **Overview** → auto-calculates 6 tiles from the two sheets: Properties Under Management, Occupied Units, Active Tenants, Leases Ending Soon, Monthly Rent Roll, Monthly Mgmt Fees
- **Properties** → list with search + filter (occupied/type) + CSV export
- **Tenants** → list with search + status filter + CSV export
- **Owner Reports** → placeholder (phase 2 — build when you have income/expense data)
- **Files** → static PDF cards (currently placeholders for the 4 templates: Management Agreement, Owner Onboarding, Tenant Application, Lease Template)

**Autocomplete**: When adding a tenant, the property address field auto-suggests from the loaded Properties list. Type the first few characters and pick — keeps addresses spelled consistently across sheets.

**Cross-sheet linking**: Tenants reference properties by `property_address` (free text). Keep the address spelling identical to what's in the Properties sheet so tiles and filters work correctly.

---

## Editing existing rows

Forms only ADD. To update a property's rent, occupancy, or notes — edit the row directly in the Google Sheet. Same for tenants. Next dashboard load reflects it.
