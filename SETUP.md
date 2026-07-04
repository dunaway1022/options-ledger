# Options Ledger — Setup Guide

This turns your options tracker into a home-screen app on your Android phone that
reads and writes live to a Google Sheet. Three parts: (1) get a Google OAuth
Client ID, (2) put the app online, (3) install it on your phone. About 15 minutes
total, one-time.

---

## Part 1 — Create a Google OAuth Client ID (~5 min)

This is what lets the app ask Google's permission to read/write *your* sheet.
Nothing goes through any third-party server — it talks directly to Google.

1. Go to **[console.cloud.google.com](https://console.cloud.google.com/)** and sign in.
2. Top left, click the project dropdown → **New Project**. Name it anything
   (e.g. "Options Ledger") → **Create**.
3. In the search bar, type **"Google Sheets API"** → open it → click **Enable**.
4. In the left sidebar: **APIs & Services → OAuth consent screen**.
   - User type: **External** → Create.
   - Fill in app name ("Options Ledger"), your email for support and developer
     contact. Save and continue through the rest (you can skip scopes/test
     users screens — click through defaults, then **Back to Dashboard**).
   - On the consent screen summary, click **Publish App** (or add yourself
     under "Test users" if you'd rather keep it unpublished — either works
     since only you will use it).
5. Left sidebar: **APIs & Services → Credentials → Create Credentials →
   OAuth client ID**.
   - Application type: **Web application**.
   - Name: anything.
   - Under **Authorized JavaScript origins**, add the URL you'll host the app
     at (see Part 2 — if using GitHub Pages it looks like
     `https://YOUR-USERNAME.github.io`). You can come back and add this after
     Part 2 if you don't have it yet.
   - Click **Create**. Copy the **Client ID** (ends in
     `.apps.googleusercontent.com`) — you'll paste this into the app.

---

## Part 2 — Put the app online (~5 min)

The app is a few static files (`index.html`, `manifest.json`, `sw.js`, two
icons). Google's sign-in requires it be served over `https://`, so it needs a
real URL — it can't just be opened from a local file. The easiest free option
is **GitHub Pages**:

1. Create a free GitHub account if you don't have one: [github.com](https://github.com).
2. Create a new repository (e.g. `options-ledger`), public, and upload all 5
   files from this folder into it (drag-and-drop on the repo's "Add file →
   Upload files" page works fine).
3. Go to the repo's **Settings → Pages**. Under "Build and deployment", set
   **Source: Deploy from a branch**, branch **main**, folder **/ (root)** →
   **Save**.
4. After a minute, your app is live at
   `https://YOUR-USERNAME.github.io/options-ledger/`.
5. Go back to Part 1, step 5, and add that exact URL (without a trailing
   path after the repo name is fine either way) to **Authorized JavaScript
   origins** as `https://YOUR-USERNAME.github.io`. Save.

(Netlify Drop or Vercel work too if you'd rather not use GitHub — any static
host that gives you an `https://` URL is fine. Just use that URL as the
authorized origin in Part 1.)

---

## Part 3 — Install it on your phone (~1 min)

1. On your Android phone, open **Chrome** and go to your app's URL.
2. Paste the **Client ID** from Part 1 when asked, sign in with Google, then
   choose **"Create a new tracker sheet"** — this builds a Google Sheet with
   the same columns as your original tracker (Trades + Monthly Tracking
   tabs), sitting in your Google Drive.
3. Tap Chrome's menu (⋮) → **Add to Home screen** → **Install**. It now
   behaves like a normal app — its own icon, full screen, no browser bar.

That's it. Every trade or month you add updates the Google Sheet instantly,
and anything you edit directly in Google Sheets (from a laptop, say) shows up
next time you open or pull-to-refresh the app.

---

## Notes

- **Your data lives entirely in your own Google Sheet** — the app itself
  stores nothing except your Client ID and Sheet ID, in your phone's local
  browser storage.
- If you'd rather use an **existing** Google Sheet instead of creating a new
  one, first duplicate your tracker into Google Sheets (File → Save as
  Google Sheets, from Google Drive, after uploading the .xlsx), keep the tab
  names **"Trades"** and **"Monthly Tracking"**, and paste its ID (the long
  string in the sheet's URL between `/d/` and `/edit`) into the "Use this
  sheet" field.
- Formulas: the app calculates Break-even, Days, Profit, Total Profit and
  %Profit/Day itself using the same logic as your spreadsheet, and writes
  the results as plain numbers — so the sheet stays fully readable even
  outside the app.
- Tax/tithing rates for the Monthly tab are set under the app's **Settings**
  tab (defaults to 25% / 10%, matching your original sheet).
- If sign-in ever stops working, it's almost always the **Authorized
  JavaScript origins** in Part 1 not exactly matching your hosted URL —
  double check for `https://` vs `http://` and no trailing slash.
