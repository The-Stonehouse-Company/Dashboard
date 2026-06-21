# Clients CRM — Setup

The Sales → **Clients** subtab renders a searchable client database with category + status filters and a CSV export for Mailchimp / Constant Contact email blasts. Open House Sign-In visitors auto-merge in as read-only entries tagged "Open House Visitor".

Three things to wire up:
1. **Stonehouse Clients** Google Sheet — where contact records live.
2. **Add Client** Google Form — what the modal POSTs to.
3. (Optional) **Open House master sheet ID** — auto-merge OH sign-ins.

Plan ~20 minutes.

---

## 1 · Create the Sheet

1. [sheets.new](https://sheets.new) (logged in as `thestonehousecompanyadmin@gmail.com`).
2. Rename: **Stonehouse Clients**
3. Share → Anyone with the link → Viewer

---

## 2 · Create the Form

1. [forms.new](https://forms.new)
2. Name it: **Add Client**
3. Add these questions in order (exact labels — lowercase, underscores):

   | # | Label | Type | Required |
   |---|---|---|---|
   | 1  | first_name        | Short answer | Yes |
   | 2  | last_name         | Short answer | Yes |
   | 3  | spouse            | Short answer | No |
   | 4  | status            | Dropdown: New / Nurture / Engaged / In Contract / Closed / Inactive | No |
   | 5  | cat_past          | Multiple choice: Yes / No | No |
   | 6  | cat_active        | Multiple choice: Yes / No | No |
   | 7  | cat_sphere        | Multiple choice: Yes / No | No |
   | 8  | cat_prospect      | Multiple choice: Yes / No | No |
   | 9  | cat_customrep     | Multiple choice: Yes / No | No |
   | 10 | email             | Short answer | No |
   | 11 | phone             | Short answer | No |
   | 12 | source            | Dropdown: Referral / Website / Open House / Social / Past Client / Sphere / Other | No |
   | 13 | address_line_1    | Short answer | No |
   | 14 | address_line_2    | Short answer | No |
   | 15 | city              | Short answer | No |
   | 16 | state             | Short answer | No |
   | 17 | zip               | Short answer | No |
   | 18 | neighborhood      | Short answer | No |
   | 19 | birthday          | Date | No |
   | 20 | closing_date      | Date | No |
   | 21 | last_touch        | Date | No |
   | 22 | next_action       | Short answer | No |
   | 23 | notes             | Paragraph | No |

4. **Settings → Responses → Link to Sheet → "Stonehouse Clients"**
5. Send → Link → Copy → change `/viewform` to `/formResponse` — that's `CLIENTS_CONFIG.FORM_URL`

---

## 3 · Get entry IDs

1. Form → ⋮ → "Get pre-filled link"
2. Fill placeholder values in every question
3. "Get link" → copy
4. Extract every `entry.NNN` and paste into `CLIENTS_CONFIG.FIELD_MAP` in `index.html`

---

## 4 · Optional — Wire Open House auto-merge

If you want Open House sign-ins to appear in the Clients list automatically (tagged "Open House Visitor", read-only):

1. Find your Open House master sheet ID (the per-agent sign-in sheet).
2. Paste it into `CLIENTS_CONFIG.OPEN_HOUSE_SHEET_ID`.

The merger looks for columns named **Full Name**, **Email**, **Phone**, and **Property Address**. Adjust the tab name via `OPEN_HOUSE_TAB` if it's not "Form Responses 1".

To skip OH auto-merge entirely, leave `OPEN_HOUSE_SHEET_ID` blank. You can still manually add notable OH visitors via the Add Client modal.

---

## 5 · Backfill existing contacts

A CSV of your existing 60 contacts (from the Numbers export you sent) is ready at:

```
The Stonehouse Company/Client Write Ups/stonehouse-clients-import.csv
```

The columns match the new schema exactly. To backfill:

1. Open the **Stonehouse Clients** sheet.
2. Go to the **"Form Responses 1"** tab.
3. Open `stonehouse-clients-import.csv` in a text editor or Numbers/Excel.
4. Copy all rows (including the header row if the sheet is blank, or just data rows if headers are already there).
5. Paste into the sheet starting at row 2 (under the headers).

The mapping I applied to your existing data:
- **Past clients** → `cat_past = Yes`
- **Active clients** → `cat_active = Yes`
- **Sphere of influence** → `cat_sphere = Yes`
- Combined tags (e.g., "Active clients,Past clients") → both flags set
- Phone numbers normalized to `(XXX) XXX-XXXX` format
- Zip codes cleaned (no trailing `.0`)
- Title/Company merged into Notes

Refresh the dashboard after pasting → all 60 contacts appear in the Clients list with filtering by category/status.

---

## How it behaves

- **Add Client modal** → row appended to the sheet (next reload picks it up).
- **Search box** → matches name / email / phone / city / neighborhood.
- **Category filter** → dropdown of: Past / Active / Sphere / Open House Visitor / Prospect / Custom Home Rep.
- **Status filter** → dropdown of: New / Nurture / Engaged / In Contract / Closed / Inactive.
- **Export CSV** → downloads the currently-filtered list. Click it after filtering to Active Clients for example → you get just those rows ready to import into Mailchimp.
- **Open House Visitors** show with a dashed pill and a left border accent to distinguish them from direct entries. They're read-only — edit at the source.

## Editing existing rows

Form only ADDS. To update a row's status, last_touch, etc., edit the row directly in the sheet. Next dashboard load reflects it.
