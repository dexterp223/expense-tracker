# Expense Tracker — family edition

A shared family expense tracker in Philippine pesos. GitHub Pages hosts the page for free,
and a free Google Firebase database keeps everyone's entries in sync. Only the Google
accounts you list can open the data.

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The app: Ledger, Dashboard, Import, Categories, Backup |
| `firebase-config.js` | Your Firebase connection settings (you fill this in, step 2) |
| `firestore.rules` | Who is allowed in: your family's Google accounts (step 2) |
| `README.md` | These instructions |

Setup takes about 20 minutes, once. You need a Google account and a free GitHub account.

---

## Step 1 — Put the files on GitHub

1. Go to https://github.com and sign in (or click **Sign up** to create a free account).
2. Click **+** (top right) → **New repository**.
   - Repository name: `expense-tracker`
   - Choose **Public**. (Free GitHub Pages needs a public repository. That's safe: the page
     contains no expenses. All data stays in Firebase behind sign-in.)
   - Click **Create repository**.
3. On the new repository's page, click the link **uploading an existing file**.
4. Drag in `index.html`, `firebase-config.js`, `firestore.rules` and `README.md`.
   Click **Commit changes**.
5. Go to **Settings** → **Pages** (left menu).
   - Source: **Deploy from a branch**
   - Branch: **main**, folder **/ (root)** → **Save**
6. Wait 1–2 minutes and refresh. Your site's address appears at the top, like
   `https://YOUR-USERNAME.github.io/expense-tracker/`. Write it down.

Opening it now shows "This copy isn't connected to a database yet". That's expected until step 2.

## Step 2 — Create the Firebase database

1. Go to https://console.firebase.google.com and sign in with your Google account.
2. Click **Create a project**. Name it `family-expenses`, continue, and turn **off**
   Google Analytics (not needed). Click **Create project**.
3. **Turn on Google sign-in:** left menu **Build → Authentication** → **Get started** →
   **Google** → **Enable** → choose your email as the support email → **Save**.
4. **Allow your GitHub site to sign in:** still in Authentication, open the **Settings** tab →
   **Authorized domains** → **Add domain** → type `YOUR-USERNAME.github.io` (no `https://`,
   no `/expense-tracker`) → **Add**.
5. **Create the database:** left menu **Build → Firestore Database** → **Create database** →
   location **asia-southeast1 (Singapore)** → **Start in production mode** → **Create**.
6. **Set who is allowed in:** in Firestore, open the **Rules** tab. Delete everything there,
   paste the whole contents of `firestore.rules`, and edit the email list:
   ```
   'dexterp0223@gmail.com',
   'spouse@gmail.com',
   'child@gmail.com'
   ```
   Every address in lowercase, in quotes, separated by commas (no comma after the last one).
   Click **Publish**.
7. **Get the connection settings:** click the gear ⚙ next to *Project Overview* →
   **Project settings** → scroll to **Your apps** → click the **</>** (Web) icon →
   nickname `expense-tracker` → **Register app** (leave "Firebase Hosting" unticked).
   Firebase shows a block like:
   ```js
   const firebaseConfig = {
     apiKey: "AIza…",
     authDomain: "family-expenses-1234.firebaseapp.com",
     projectId: "family-expenses-1234",
     ...
   };
   ```
8. **Put them in `firebase-config.js`:** on GitHub, open `firebase-config.js` → click the
   ✏️ pencil (Edit) → replace each `PASTE_…` value with the matching value from Firebase →
   **Commit changes**.
9. Wait a minute, then open your site address. Click **Sign in with Google**. 🎉

## Step 3 — Move your existing data over

1. Open your claude.ai Expense Tracker → **Backup** → **Download backup file**.
2. Open the GitHub version → sign in → **Backup** → **Restore from a backup file** → choose that
   file → **Restore**. Your expenses, categories, colors and budget come across.

## Step 4 — Invite the family

1. Add each person's Gmail address to the rules (step 2.6) and click **Publish**.
2. Send them your site address. They click **Sign in with Google** and see the same ledger.
   - Someone without Gmail (for example a Yahoo address) can create a free Google account
     using their existing email at https://accounts.google.com/signup → "Use my current email
     address instead". Add that email to the rules.
   - Anyone not on the list sees "isn't on the family list" and can't read anything.

## Optional — Statement reading on the Import page

Reading credit card statements uses Claude through the Anthropic API, which is pay-per-use.
1. Create a key at https://console.anthropic.com → **API Keys** → **Create Key**, and add a
   small amount of credit under **Billing**. Set a monthly spend limit there if you like.
2. In the app: **Backup** page → **Statement reading** → paste the key → **Save key**.

The key is saved only in that browser, so it isn't shared with the family or put on GitHub.
Each person who wants to import statements adds a key on their own device. Without a key,
everything else works; only the Import page's **Read statement** is off.

## Differences from the claude.ai version

- **Same:** Ledger, Dashboard, categories and colors, card banks, edit entries, double-entry
  check, CSV export, backup download and restore.
- **Shared:** everyone on the family list sees and edits the same data, live.
- **Not included:** one-click Google Drive backup and "Save to Google Sheets". Use
  **Download backup file** / **Download CSV** and upload the file to Drive yourself.
- **The monthly email report** set up in the Claude app reads the claude.ai version's data, not
  this Firebase database.

## Updating the app later

Replace `index.html` in the repository (**Add file → Upload files**, same name, **Commit**).
Don't overwrite `firebase-config.js` once it's filled in.

## Costs

GitHub Pages and Firebase's free (Spark) plan cost nothing at family scale: Firestore's free tier
allows 50,000 reads and 20,000 writes per day. Only statement reading (optional) costs money.
