# PAVOI Inspire — Setup Guide

Complete this guide once and the system runs forever at zero cost.
Estimated setup time: **30–45 minutes**.

---

## What you're building

| File | Purpose |
|------|---------|
| `pavoi-capture.html` | Mobile PWA — capture inspiration photos and URLs |
| `pavoi-synthesis.html` | Team digest, blind voting, GO/DISCUSS/ARCHIVE |
| `pavoi-bookmarklet.html` | Desktop bookmarklet setup page |
| `SETUP.md` | This guide |

**Stack:** GitHub Pages (hosting) · Google Forms + Sheets (database) · ImgBB (image hosting)
**Cost:** $0 · No server · No maintenance

---

## Part 1 — Google Form (Inspiration Captures)

### 1.1 Create the form

1. Go to [forms.google.com](https://forms.google.com) → New blank form
2. Name it: **PAVOI Inspire — Captures**
3. Add the following fields **in this exact order**:

| # | Field Name | Type | Required |
|---|-----------|------|----------|
| 1 | URL | Short answer | No |
| 2 | Saved By | Short answer | Yes |
| 3 | Save Date | Short answer | Yes |
| 4 | Note | Paragraph | Yes |
| 5 | Image URL | Short answer | No |
| 6 | Platform | Short answer | No |
| 7 | Content Type | Short answer | No |
| 8 | Brand | Short answer | No |
| 9 | Price | Short answer | No |
| 10 | Tags | Short answer | No |
| 11 | Urgent | Short answer | No |

### 1.2 Critical form settings

In the form, click the ⚙️ **Settings** gear:
- **"Collect email addresses"** → set to **Off** (or "Responder input" — NOT "Verified")
- **"Limit to 1 response"** → **Off**
- **"Require sign in"** → **Off**

> ⚠️ If these are wrong, submissions are silently rejected with no error message.

### 1.3 Get the Form ID

Your form URL looks like:
```
https://docs.google.com/forms/d/1AbCdEfGhIjKlMnOpQrStUvWxYz/edit
```
The part between `/d/` and `/edit` is your **Form ID**: `1AbCdEfGhIjKlMnOpQrStUvWxYz`

### 1.4 Find the entry field IDs

This is the most important step. Field IDs are like `entry.1234567890`.

1. Open your form in edit mode
2. Click the **⋮ menu** (top right) → **"Get pre-filled link"**
3. Enter dummy text in every field (e.g., "test") → click **"Get Link"** → copy the URL
4. The URL will look like:
   ```
   https://docs.google.com/forms/d/YOUR_ID/viewform?entry.111111111=test&entry.222222222=test&...
   ```
5. Map each `entry.XXXXXXXXX` to the field it corresponds to based on order

Open `pavoi-capture.html` in a text editor and fill in the `CONFIG` block at the top:

```javascript
const CONFIG = {
  GOOGLE_FORM_ID: '1AbCdEfGhIjKlMnOpQrStUvWxYz',  // ← your form ID
  FIELD_SOURCE_URL:    'entry.111111111',            // ← from step above
  FIELD_SAVED_BY:      'entry.222222222',
  FIELD_SAVE_DATE:     'entry.333333333',
  FIELD_NOTE:          'entry.444444444',
  FIELD_IMAGE_URL:     'entry.555555555',
  FIELD_PLATFORM:      'entry.666666666',
  FIELD_CONTENT_TYPE:  'entry.777777777',
  FIELD_BRAND:         'entry.888888888',
  FIELD_PRICE:         'entry.999999999',
  FIELD_TAGS:          'entry.101010101',
  FIELD_URGENT:        'entry.111111112',
  // ...
};
```

---

## Part 2 — Google Form (Votes)

Repeat Part 1 for a second, separate form for team votes.

### 2.1 Create the Votes form

Name it: **PAVOI Inspire — Votes**

Fields in order:

| # | Field Name | Type | Required |
|---|-----------|------|----------|
| 1 | Voter Name | Short answer | Yes |
| 2 | Item ID | Short answer | Yes |
| 3 | Vote | Short answer | Yes |
| 4 | Week | Short answer | Yes |

Same settings as above: email off, limit off, sign-in off.

Get the Form ID and field entry IDs the same way.

Fill in `pavoi-synthesis.html` CONFIG:

```javascript
VOTE_FORM_ID:      '1XxXxXxXxXxXxXxXxXxXxXxX',
VOTE_FIELD_VOTER:  'entry.111111111',
VOTE_FIELD_ITEM:   'entry.222222222',
VOTE_FIELD_VOTE:   'entry.333333333',
VOTE_FIELD_WEEK:   'entry.444444444',
```

---

## Part 3 — Google Sheets (publish as CSV)

### 3.1 Inspiration Sheet

When users submit the form, Google auto-creates a Sheet linked to the form.

1. Open the form → **Responses** tab → click the green **Sheets icon**
2. This creates a linked spreadsheet
3. In the spreadsheet: **File → Share → Publish to web**
4. Under "Link", select your sheet tab → select **CSV** format → click **Publish**
5. Copy the URL — it looks like:
   ```
   https://docs.google.com/spreadsheets/d/1ShEeTiD/pub?output=csv
   ```

Paste this into `pavoi-synthesis.html` CONFIG:
```javascript
SHEET_INSPIRATION_CSV: 'https://docs.google.com/spreadsheets/d/1ShEeTiD/pub?output=csv',
```

### 3.2 Votes Sheet

Do the same for the Votes form's linked sheet:
```javascript
SHEET_VOTES_CSV: 'https://docs.google.com/spreadsheets/d/1VoTeShEeT/pub?output=csv',
```

> **Note:** Published CSV has a ~1–5 minute cache delay. This is fine for weekly digest use. Tap ↻ Refresh to force a reload.

---

## Part 4 — ImgBB (image hosting)

The capture app uploads photos to ImgBB (free, permanent, no account required for users).

1. Go to [imgbb.com/api](https://api.imgbb.com/)
2. Create a free account
3. Copy your API key
4. Paste it into `pavoi-capture.html` CONFIG:

```javascript
IMGBB_API_KEY: 'abc123yourkeyhere',
```

ImgBB free tier: unlimited uploads, permanent URLs. No CDN limits.

---

## Part 5 — GitHub Pages (hosting)

### 5.1 Create the repository

1. Go to [github.com/new](https://github.com/new)
2. Name: `pavoi-inspire` (or anything you like)
3. Set to **Public** (required for free GitHub Pages)
4. Click **Create repository**

### 5.2 Upload the files

Upload all four files:
- `pavoi-capture.html`
- `pavoi-synthesis.html`
- `pavoi-bookmarklet.html`
- `SETUP.md`

Via the GitHub UI: click **Add file → Upload files**, drag all four in, commit.

### 5.3 Enable GitHub Pages

1. Go to the repo **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** · Folder: **/ (root)**
4. Click **Save**

Your site will be live at:
```
https://YOUR_USERNAME.github.io/pavoi-inspire/
```

Give it 2–3 minutes to deploy. Test:
- `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-capture.html`
- `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-synthesis.html`
- `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-bookmarklet.html`

### 5.4 Update the bookmarklet URL

Open `pavoi-bookmarklet.html` and update:
```javascript
const CAPTURE_URL = 'https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-capture.html';
```
Commit and push. The bookmarklet will now point to the correct live URL.

---

## Part 6 — iOS PWA Installation

The capture and digest apps work as Home Screen apps on iPhone.

### For each team member (do once):

**Capture app:**
1. Open Safari (must be Safari — not Chrome)
2. Go to: `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-capture.html`
3. Tap the **Share** button (box with arrow) → **Add to Home Screen**
4. Name it: **PAVOI Capture** → **Add**

**Digest app:**
1. Repeat with: `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-synthesis.html`
2. Name it: **PAVOI Digest** → **Add**

> ⚠️ **Important:** When files are updated and pushed to GitHub, iOS PWA users must **reinstall from Safari** (delete old icon, re-add). iOS caches aggressively. Tell the team to reinstall whenever you push an update.

---

## Part 7 — Desktop Bookmarklet

1. Open `pavoi-bookmarklet.html` in Chrome or Safari (local or from GitHub Pages)
2. Show your bookmarks bar: **⌘⇧B** (Chrome) or **View → Show Favorites Bar** (Safari)
3. Drag the gold **✦ PAVOI Capture** button to your bookmarks bar
4. Done — click it on any website to capture

If drag doesn't work: click "Copy bookmarklet code" → create a new bookmark manually → paste the code as the URL.

---

## Part 8 — Team Onboarding

Send this message to the team:

---

**PAVOI Inspire is live!**

We now have a system for capturing and voting on inspiration as a team. Here's what to do:

**On your phone (install as apps):**
- Capture app: `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-capture.html`
  → Safari → Share → Add to Home Screen → "PAVOI Capture"
- Digest app: `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-synthesis.html`
  → Safari → Share → Add to Home Screen → "PAVOI Digest"

**On desktop:**
- Visit `https://YOUR_USERNAME.github.io/pavoi-inspire/pavoi-bookmarklet.html` for the one-click bookmarklet setup

**The workflow:**
1. Save inspiration any time — photos from camera roll, or Copy Link from Instagram/TikTok → Paste in the app
2. Add a note explaining what you're asking the team to look at
3. Every Friday (or when ready): open Digest → vote YES/NO on each item by swiping
4. Results auto-calculate: ≥70% = GO · 40–69% = DISCUSS · <40% = Archive
5. Export a Slack digest or Claude prompt when done

---

## Part 9 — Managing the Team

To add or remove team members, edit the `TEAM` array in **both** `pavoi-capture.html` and `pavoi-synthesis.html`:

```javascript
TEAM: [
  'Hannah Samlin',
  'Rita Lopes',
  'Tal Masica',
  'Shynade Austin',
  'Tara Ganon',
  'Kelly Hallinan',
  'New Person'    // ← add here
],
```

After editing, commit and push to GitHub. Old vote data is preserved — new member just starts from the current week forward.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Submissions not appearing in Sheet | Check form settings: email off, limit off, sign-in off |
| ImgBB upload fails | Verify API key in CONFIG. Free tier limit: 32MB per image. |
| Sheet not loading in Digest | Make sure sheet is published (File → Share → Publish to web → CSV). Check SHEET_INSPIRATION_CSV URL. |
| CORS error loading CSV | The app auto-falls back to corsproxy.io. If that fails, re-publish the sheet. |
| iOS app shows black screen | All CSS must use hex colors (no CSS variables). This is already handled in these files. |
| iOS app not updating after edit | Delete the Home Screen icon, go to GitHub Pages URL in Safari, re-add to Home Screen. |
| Bookmarklet URL says YOUR_USERNAME | Update `CAPTURE_URL` in `pavoi-bookmarklet.html` before deploying. |
| Swipe voting not registering | Make sure you're swiping at least 80px horizontally. Tap 👍/👎 buttons as alternative. |
| Votes not showing in Results | Votes sheet may have a 1–5 min cache delay. Tap ↻ Refresh. |

---

## Summary Checklist

- [ ] Created Inspiration Google Form (11 fields, email off, limit off)
- [ ] Created Votes Google Form (4 fields, same settings)
- [ ] Got Form IDs and entry field IDs for both forms
- [ ] Published both linked Sheets as CSV
- [ ] Got ImgBB API key
- [ ] Filled in all CONFIG values in `pavoi-capture.html`
- [ ] Filled in all CONFIG values in `pavoi-synthesis.html`
- [ ] Updated `CAPTURE_URL` in `pavoi-bookmarklet.html`
- [ ] Created GitHub repo, uploaded all 4 files, enabled GitHub Pages
- [ ] Tested a capture submission (check it appears in the Sheet)
- [ ] Installed capture + digest apps on your iPhone
- [ ] Sent onboarding instructions to team
- [ ] Set up weekly Friday digest ritual 🎉

---

*Questions? The CONFIG blocks at the top of each HTML file have comments explaining every field.*
