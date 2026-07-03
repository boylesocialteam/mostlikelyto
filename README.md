# Most Likely To 🎉

A party game for you and your friends. Everyone opens the same web link on their own phone, joins a room with a 4-letter code, and votes on who's *most likely to…* — results reveal live on every screen at once.

Built as a plain static website (HTML + JavaScript) hosted free on **GitHub Pages**, syncing phones through a free **Firebase Realtime Database**. No server to run, no developer needed. Comfortable for small groups (built and tested for ~4–6 players).

---

## What's in here

| File | What it does |
|---|---|
| `index.html` | The whole game — UI and logic |
| `questions.js` | The question library (267 questions). **Edit this to add your own.** |
| `firebase-config.js` | Your Firebase keys go here (one-time setup) |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is |

---

## Setup — two steps, about 15 minutes total, free

### Step 1 — Set up Firebase (the live sync)

1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **Add project**, give it any name (e.g. `most-likely-to`), and create it. You can skip Google Analytics.
3. On the project dashboard, click the **web icon** `</>` to "Add app to get started". Give it a nickname, click **Register app**. **Do not** tick "Firebase Hosting".
4. Firebase shows you a `firebaseConfig = { … }` block. Copy those values.
5. In the left menu go to **Build → Realtime Database → Create Database**. Pick a location, and choose **Start in test mode** (fine for a private game — you can lock it down later, see below). Click Enable.
6. Open **`firebase-config.js`** in this project and paste your values over the `PASTE_…` placeholders. Make sure `databaseURL` is filled in — you'll find it at the top of the Realtime Database page (looks like `https://your-project-default-rtdb.firebaseio.com`).
7. Save the file.

That's the only file you edit for setup.

### Step 2 — Publish to GitHub Pages (the shareable link)

**Easiest (browser only, no git):**

1. Create a free account at **https://github.com** if you don't have one.
2. Click **New repository**, name it e.g. `most-likely-to`, set it **Public**, click **Create**.
3. On the new repo page click **uploading an existing file**, then drag in **all the files from this folder** (`index.html`, `questions.js`, `firebase-config.js`, `.nojekyll`, `README.md`). Click **Commit changes**.
4. Go to the repo's **Settings → Pages**. Under "Build and deployment", set **Source = Deploy from a branch**, **Branch = main**, folder **/(root)**, and **Save**.
5. Wait ~1 minute, then refresh. GitHub shows your live link, like `https://yourname.github.io/most-likely-to/`.

**If you prefer git (command line):**

```bash
cd most-likely-to-game
git init && git add . && git commit -m "Most Likely To game"
git branch -M main
git remote add origin https://github.com/YOURNAME/most-likely-to.git
git push -u origin main
```
Then enable Pages as in step 4 above.

### Play

Open your GitHub Pages link on your phone, type your name, tap **Host a new game**, and share the 4-letter code (or tap **Copy invite link**) with your friends. They open the link, enter the code, and you're playing. The host's phone drives the game (Start / Next / Reveal).

---

## Add your own questions

Open **`questions.js`** and copy an existing line:

```js
{"q": "Most likely to text the wrong group chat.", "cat": "Social Media", "tone": "Awkward", "diff": "Medium", "deb": 7, "hum": 8},
```

- `q` — the question. Always start with "Most likely to…", keep it under ~15 words.
- `cat` — category (any label you like).
- `tone` — `Funny`, `Wholesome`, `Savage`, `Chaotic`, `Awkward`, `Competitive`, `Random`, `Clever`, `Cute`.
- `deb` / `hum` — debate and humour scores out of 10 (used only for the 🔥 badge and the "High debate" vibe filter).

Add as many lines as you want, commit/upload the file, and they're live. The master library and a spreadsheet version live in the parent project folder if you want to bulk-edit.

---

## Making it private (optional, recommended)

Test mode lets anyone with your database URL read/write for 30 days. For a private friends game that's usually fine, but to lock it to just game rooms, go to **Realtime Database → Rules** and paste:

```json
{
  "rules": {
    "rooms": {
      "$code": {
        ".read": true,
        ".write": true
      }
    },
    "stats": {
      ".read": true,
      ".write": true
    }
  }
}
```

The `stats` section is required for the **Group Stats** page (all-time votes, round wins, and answer history). If you tightened your rules earlier to rooms-only, re-paste the block above and Publish, or the stats won't save.

This keeps writes scoped to room data. Note: because it's a client-only game, a technically-minded player could in theory peek at votes in the database before the reveal — for a casual game among friends this isn't a concern, but it's why this is best kept to people you trust. True vote-hiding would need a small server, which this deliberately avoids.

---

## Costs

Everything here runs on free tiers: GitHub Pages is free for public repos, and Firebase's free **Spark** plan includes 1 GB storage, 10 GB/month transfer and 100 simultaneous connections — a few friends won't come close.

*Free-tier limits and Firebase SDK version (12.15.0) are current as of July 2026; verify if you're reading this much later.*
