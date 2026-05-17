# Push the Dashboard to GitHub — Step-by-Step

You wanted this in a **private repo under the `The-Stonehouse-Company` org**. Here's the full one-time setup. Everything happens locally on your Mac, then pushes up to GitHub.

You'll use the admin GitHub account (`stonehousecompanyadmin`) — which owns the org — to create and push the repo.

---

## Step 1 — Create the private repo on GitHub.com

1. Make sure you're signed into GitHub as **`stonehousecompanyadmin`**.
2. Go to **https://github.com/organizations/The-Stonehouse-Company/repositories/new**.
3. Fill in:
   - **Owner:** `The-Stonehouse-Company`
   - **Repository name:** `dashboard`
   - **Description:** `Public-facing company hub for The Stonehouse Company.`
   - **Visibility:** **Private** ← important
   - **Do NOT** check "Add a README", "Add .gitignore", or "Choose a license" — we already have these locally.
4. Click **Create repository**.

GitHub will show you a page that says "Quick setup" with a URL like:

```
https://github.com/The-Stonehouse-Company/dashboard.git
```

Copy that URL — you'll need it in step 3.

---

## Step 2 — Initialize git locally and make the first commit

Open **Terminal** and run, **line by line** (the path has spaces, so the quotes matter):

```bash
cd "/Users/ryanstonehouse/The Stonehouse Company/The Stonehouse Company/Company/Dashboard"

# If you've never used git on this Mac, tell git who you are first:
git config --global user.name  "Ryan Stonehouse"
git config --global user.email "thestonehousecompanyadmin@gmail.com"

# Initialize the repo
git init -b main

# Stage and commit everything
git add .
git commit -m "Initial dashboard: brand-matched company hub + Apps Script doGet docs"
```

---

## Step 3 — Connect to GitHub and push

Still in the same Terminal window:

```bash
# Replace the URL below with the one you copied in step 1 if it differs
git remote add origin https://github.com/The-Stonehouse-Company/dashboard.git
git push -u origin main
```

The first time you push, GitHub will ask you to authenticate. The cleanest way:

### Authentication option A — GitHub CLI (recommended)

```bash
# Install once
brew install gh

# Sign in (choose "GitHub.com", then "HTTPS", then "Login with a web browser")
gh auth login

# Now push
git push -u origin main
```

When `gh auth login` opens your browser, **make sure you sign in as `stonehousecompanyadmin`**, not your personal account.

### Authentication option B — Personal access token

1. While signed in as `stonehousecompanyadmin`, go to **https://github.com/settings/tokens?type=beta**.
2. Click **Generate new token**.
3. Resource owner: **The-Stonehouse-Company** (so the token can write to the org).
4. Repository access: **Only select repositories → dashboard**.
5. Permissions → Repository → **Contents: Read and write**.
6. Generate, copy the token.
7. Back in Terminal, run `git push -u origin main`. When prompted:
   - **Username:** `stonehousecompanyadmin`
   - **Password:** paste the token (it won't show as you type — that's normal).

---

## Step 4 — Verify

1. Refresh `https://github.com/The-Stonehouse-Company/dashboard`.
2. You should see `index.html`, `README.md`, `assets/`, and `docs/` — and a 🔒 padlock next to the repo name confirming it's private.

---

## Day-to-day workflow

After the one-time setup, every change is just:

```bash
cd "/Users/ryanstonehouse/The Stonehouse Company/The Stonehouse Company/Company/Dashboard"
git add .
git commit -m "Brief description of what changed"
git push
```

---

## A note on the `SHEETS_API_URL`

The `/exec` URL from Apps Script is not a true secret (anyone with the link can fetch the aggregate JSON), but it's still cleaner to keep it out of public-facing repos. **This repo is private**, so it's fine to paste the URL directly into `index.html`. If you ever change visibility to public, move that URL out of source — see `.gitignore`, which already excludes `config.local.js` and `.env` files for that purpose.
