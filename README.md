# Unified Apps Hub (GitHub Pages)

One repository that publishes **four complete client-side apps** as a single GitHub Pages website:

| App | Path | Type | Notes |
|-----|------|------|--------|
| **Portal** | `/` | Static HTML | Landing page with links to all apps |
| **Local Event Creator** | `/event-creator/` | Static HTML | GPS, photo, map, localStorage, share |
| **Al-Rahma Hub** | `/al-rahma-hub/` | Static HTML | Onboarding, loyalty stamps, cookies |
| **Quantum Stego Suite** | `/stego-crypt/` | Static HTML + PWA | AES-GCM, QR keys, LSB steganography |
| **Travel to Russia Abroad** | `/travel-russia/` | React (Vite) + PWA | **2,800-word** vocabulary, flashcards, quiz, etc. |

**Nothing was removed.** The vocabulary file (`travel-russia/src/data/vocabulary.json`) is used **as-is** (exactly 2,800 entries). No rewrite.

---

## Quick deploy (recommended)

### 1. Create the repository

1. Go to [https://github.com/new](https://github.com/new)
2. Name it (example: `unified-apps-hub`)
3. Choose **Public**
4. **Do not** add a README, .gitignore, or license (you already have them)
5. Click **Create repository**

### 2. Upload this folder

**Option A — GitHub web UI**

1. On the empty repo page click **uploading an existing file**
2. Drag **everything inside** the `unified-site` folder (including `.github`, `index.html`, and the four app folders)
3. Commit to the `main` branch

**Option B — Git command line**

```bash
cd unified-site
git init
git add .
git commit -m "Initial unified apps site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
git push -u origin main
```

### 3. Enable GitHub Pages with Actions

1. Open the repository on GitHub
2. Go to **Settings → Pages**
3. Under **Build and deployment → Source**, choose **GitHub Actions**
4. Go to the **Actions** tab
5. Open the workflow **Deploy Unified Site to GitHub Pages**
6. Wait for the green checkmark (first run builds the React travel app)

### 4. Open your site

After the workflow finishes, the site URL will look like:

```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
```

From the portal you can open:

- Event Creator → `.../event-creator/`
- Al-Rahma Hub → `.../al-rahma-hub/`
- StegoCrypt → `.../stego-crypt/`
- Travel to Russia → `.../travel-russia/`

---

## What the GitHub Action does

On every push to `main` (and when you click “Run workflow”):

1. Installs Node.js 22 and the travel-russia npm packages
2. Runs `vite build` with **relative base** (`base: './'`) so assets work in the subfolder
3. Copies:
   - Portal `index.html`
   - `event-creator/`, `al-rahma-hub/`, `stego-crypt/` (unchanged static files)
   - Built files from `travel-russia/dist/` → `site/travel-russia/`
4. Uploads the assembled `site/` folder as a GitHub Pages artifact
5. Deploys it

You never need to run a local build for deployment.

---

## Local development (optional)

### Static apps

Just open the HTML files in a browser, or serve the folder:

```bash
# from the unified-site root
npx serve .
```

### Travel to Russia (React)

```bash
cd travel-russia
npm install
npm run dev
```

Then open the URL shown in the terminal (usually `http://localhost:5173`).

To test the production build locally:

```bash
cd travel-russia
npm run build
npx serve dist
```

---

## Features kept (summary)

### Local Event Creator
- Event form (name, date, time, duration, weather, description, age, venue, theme, dress code, bring list, music/other links)
- Paid event + payment URL
- High-accuracy GPS + accuracy estimate in feet
- Manual coordinates / place name
- Nudge pin N/S/E/W by configurable feet (default 3.3 ft ≈ 1 m)
- Google Maps link + embed preview
- Photo capture/resize to JPEG for localStorage
- Save / edit / delete events in browser storage
- Native share (text + image when supported)

### Al-Rahma Hub
- Splash / onboarding with captcha and profile fields
- Cookie-based persistence
- Main hub with 5-slot loyalty stamp card + reward
- Category bonus claims
- Theme toggle
- Interactive sandbox / cookie inspector
- All original client-side logic

### Quantum Stego Suite (StegoCrypt Pro)
- AES-GCM 256-bit key generation + cookie persistence
- User ID + QR code for key exchange
- Friends list via QR image upload
- Encrypt plaintext only
- LSB steganography encode into PNG
- Decode stego image + decrypt
- Export key file
- Service worker + manifest (installable PWA)

### Travel to Russia Abroad
- **2,800 vocabulary entries** (vocabulary.json untouched)
- CEFR-style levels A1 / A2 / B1 / B2
- Dashboard, course levels, flashcards, quiz, type Russian, match, listening (TTS), writing notes
- Vocabulary browser with search / topic filters
- Citizenship practice sample questions + history/law outline
- Local progress (known words, history, writing) + JSON export
- PWA via vite-plugin-pwa

---

## Requirements & notes

- **GitHub Pages** must use the **GitHub Actions** source (not “Deploy from a branch”) for the React app to be built.
- Use **HTTPS** (GitHub Pages provides it). GPS, share, camera, and some crypto features need a secure context.
- All data stays in the browser (localStorage / cookies). Nothing is sent to a server by these apps.
- The travel vocabulary file is large; the first Actions build may take a minute or two.
- Stego and Travel include service workers; hard-refresh or clear site data if you see a stale cache after updates.

---

## Repository layout

```
unified-site/
├── index.html                 # Portal
├── README.md
├── .github/workflows/deploy.yml
├── event-creator/
│   └── index.html
├── al-rahma-hub/
│   └── index.html
├── stego-crypt/
│   ├── index.html
│   ├── app.js
│   ├── style.css
│   ├── manifest.json
│   └── sw.js
└── travel-russia/             # Source (built by Actions)
    ├── package.json
    ├── vite.config.js         # base: './'
    ├── index.html
    ├── public/
    └── src/
        ├── App.jsx
        ├── main.jsx
        ├── styles.css
        └── data/
            └── vocabulary.json   # 2,800 words — do not rewrite
```

After the Action runs, the published site contains the built `travel-russia/` folder instead of the source.

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Actions fails on `npm install` | Re-run the workflow. Check that `travel-russia/package.json` was uploaded. |
| Travel app shows blank page | Confirm the Action finished successfully and you are opening `/travel-russia/` (with trailing slash). Hard-refresh. |
| GPS / share not working | Must be on the HTTPS Pages URL, not `file://`. Allow location permission. |
| Stego QR libraries fail | Needs network once to load CDN scripts (qrcodejs + html5-qrcode). |
| Want only static deploy (no Actions) | You can open Event / Rahma / Stego immediately. Travel still needs a build; keep using Actions or run `npm run build` locally and copy `dist` into `travel-russia/`. |

---

Made for GitHub Pages. All four original feature sets included.
