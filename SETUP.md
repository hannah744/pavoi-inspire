# PAVOI Inspire — Setup Guide

**Total time: ~20 minutes. Do this once.**

---

## What you're setting up

| File | Purpose |
|---|---|
| `pavoi-capture.html` | Capture form — mobile PWA + bookmarklet popup |
| `pavoi-bookmarklet.html` | Generates your browser bookmarklet |
| `pavoi-synthesis.html` | Weekly digest, voting, GO/NO-GO results |

---

## Step 1 — Create your Google Form

Go to [forms.google.com](https://forms.google.com) and create a new blank form.

Add these fields **in order** (all Short Answer unless noted):

1. **URL** *(Short Answer)*
2. **Platform** *(Dropdown: E-commerce, Instagram, TikTok, Editorial, Pinterest, Other)*
3. **Content Type** *(Dropdown: Product Photo, Product Listing, Social Post, Video, Trend Report)*
4. **Brand / Creator** *(Short Answer)*
5. **Price** *(Short Answer)*
6. **Review Score** *(Short Answer)*
7. **Review Count** *(Short Answer)*
8. **Likes** *(Short Answer)*
9. **Comments** *(Short Answer)*
10. **Saves** *(Short Answer)*
11. **Saved By** *(Short Answer)*
12. **Save Date** *(Short Answer)*
13. **Inspiration Note** *(Paragraph — this is the required field)*
14. **Aesthetic Tags** *(Short Answer)*
15. **Urgent** *(Dropdown: Yes, No)*

> The field names must match **exactly** as written above — the synthesis app uses them as column headers.

---

## Step 2 — Find your entry IDs

1. In the form editor, click **⋮ (More options) → Get pre-filled link**
2. Fill in every field with any dummy value
3. Click **"Get Link"** — copy the URL
4. Paste it into a text editor. It will look like:
   ```
   https://docs.google.com/forms/d/e/XXXXX/viewform?entry.1234567890=dummy&entry.9876543210=dummy&...
   ```
5. Each `entry.XXXXXXXXX` corresponds to a field, in the order you created them
6. Note: the form action URL = same base, but change `viewform?...` to `formResponse`

---

## Step 3 — Configure pavoi-capture.html

Open `pavoi-capture.html` in a text editor. At the top, find `PAVOI_CONFIG` and fill in:

```javascript
const PAVOI_CONFIG = {
  formAction: "https://docs.google.com/forms/d/e/YOUR_FORM_ID/formResponse",
  fields: {
    url:          "entry.XXXXXXXXX",   // ← field 1
    platform:     "entry.XXXXXXXXX",   // ← field 2
    contentType:  "entry.XXXXXXXXX",   // ← field 3
    brand:        "entry.XXXXXXXXX",   // ← field 4
    price:        "entry.XXXXXXXXX",   // ← field 5
    reviewScore:  "entry.XXXXXXXXX",   // ← field 6
    reviewCount:  "entry.XXXXXXXXX",   // ← field 7
    likes:        "entry.XXXXXXXXX",   // ← field 8
    comments:     "entry.XXXXXXXXX",   // ← field 9
    saves:        "entry.XXXXXXXXX",   // ← field 10
    savedBy:      "entry.XXXXXXXXX",   // ← field 11
    saveDate:     "entry.XXXXXXXXX",   // ← field 12
    note:         "entry.XXXXXXXXX",   // ← field 13
    tags:         "entry.XXXXXXXXX",   // ← field 14
    urgent:       "entry.XXXXXXXXX"    // ← field 15
  }
};
```

Save the file.

---

## Step 4 — Host pavoi-capture.html

The bookmarklet needs to open `pavoi-capture.html` from a URL, not a local file.

**Option A — GitHub Pages (free, ~5 min):**
1. Create a public GitHub repo (e.g., `pavoi-inspire`)
2. Upload `pavoi-capture.html`
3. Go to repo Settings → Pages → Source: main branch → Save
4. URL: `https://YOUR-ORG.github.io/pavoi-inspire/pavoi-capture.html`

**Option B — Shopify:**
1. Online Store → Themes → Edit code → Assets folder
2. Upload `pavoi-capture.html`
3. URL: `https://pavoi.com/cdn/shop/t/X/assets/pavoi-capture.html`

---

## Step 5 — Generate your bookmarklet

1. Open `pavoi-bookmarklet.html` in your browser
2. Paste:
   - Your hosted capture page URL (from Step 4)
   - Your Google Form action URL (from Step 2)
   - All 15 entry IDs
3. Click **"Generate Bookmarklet ✦"**
4. **Drag the "PAVOI ✦ Save" button to your bookmarks bar**
5. Share the same steps with each team member (they each need to add the bookmarklet)

> **Using it:** Browse to any product page, Instagram post, or editorial. Click the bookmarklet. A capture form opens with the URL and any auto-detected price/brand pre-filled. Fill in your note. Submit.

---

## Step 6 — iOS Setup (iPhone)

### Option A — Add capture page to home screen (simplest)
1. On iPhone, open Safari and navigate to your hosted `pavoi-capture.html`
2. Tap **Share → Add to Home Screen**
3. Name it **PAVOI Inspire**
4. The icon will appear on your home screen as an app
5. When you want to capture: copy the URL from whatever you're looking at, open PAVOI Inspire, paste it

### Option B — iOS Shortcut with Share Sheet (best UX)
1. Open the **Shortcuts** app → tap **+** to create new
2. Add action: **Receive** → set "Receive" to **URLs** → tick "from Share Sheet"
3. Add action: **Text** → type your full capture URL:
   `https://YOUR-CAPTURE-PAGE-URL?url=`
4. Add action: **Combine Text** → first input = the Text from step 3, second input = **Shortcut Input** (the variable)
5. Add action: **Open URLs** → input = Combined Text
6. Name the shortcut **PAVOI Save**, set a gold star icon
7. **Using it:** On any page in Safari/Instagram/TikTok, tap **Share → PAVOI Save**. The capture page opens with the URL pre-filled.

---

## Step 7 — Configure pavoi-synthesis.html

### Publish your Google Sheet as CSV
1. Open the Google Sheet linked to your form
2. **File → Share → Publish to web**
3. Choose the sheet tab, format: **Comma-separated values (.csv)**
4. Click **Publish** → copy the URL

### Add it to the synthesis app
Open `pavoi-synthesis.html` in a text editor. Find `SYNTHESIS_CONFIG` and set:

```javascript
sheetCsvUrl: "https://docs.google.com/spreadsheets/d/e/XXXXX/pub?output=csv"
```

Also make sure the `columns` object matches your exact Google Form field names.

---

## Weekly Workflow

### Team members (throughout the week)
- Use the bookmarklet on desktop to save anything from e-commerce, editorials, competitor sites
- Use the iOS Shortcut or home screen app to save from Instagram, TikTok, Pinterest
- Inspiration note is required — no skipping it

### Hannah (Friday)
1. Open `pavoi-synthesis.html`
2. Click **Load / Refresh** → Load from Sheet
3. Review the **Weekly Digest** — run the trend prompt through Claude for a full analysis
4. Generate the **Slack Digest** and post to the team channel
5. Open voting window (5 business days)

### Team members (after the digest drops)
1. Open `pavoi-synthesis.html`
2. Go to **Vote**, enter your name
3. Vote Yes/No on each item
4. Click **Export My Votes** and paste the code into Slack (DM to Hannah)

### Hannah (after voting window closes)
1. Go to **Aggregate Votes**, paste all vote export codes
2. Click **Aggregate**
3. View **Results** — assign P1/P2/P3 to GO items
4. Flag any items for Figma using **+ Figma** button
5. Copy Figma JSON → import via Content Reel or paste into Figma

---

## Troubleshooting

**"Could not fetch the Sheet"** — The app can't make cross-origin requests when running as a local `file://` page. Either host the synthesis app on GitHub Pages/Shopify, or use **Paste CSV** (download CSV from Sheets, paste the content).

**Form submission shows success but nothing in Sheet** — Check that your form action URL ends in `formResponse` (not `viewform`). Also verify the entry IDs match the correct fields.

**iOS Shortcut doesn't open the capture page** — Make sure the Combine Text action is URL-encoding the input. In the Shortcut, the variable for the page URL should be plain text, and the Shortcut Input (the URL being shared) should have "URL encode" enabled.

**Tags not auto-detecting on the bookmarklet** — Price and brand detection uses common CSS selectors and works on most e-commerce sites. It won't work on Instagram/TikTok — fill those in manually in the capture form.

---

*PAVOI Inspire — built for the design team. Questions: Hannah (hannah@pavoi.com)*
