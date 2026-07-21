# A Little Birthday Surprise 🎁

A tiny, offline-capable Progressive Web App birthday experience — gift box, balloon
wishes, cake decorating, a catch game, a wish gallery, fireworks, and a final card.
Pure HTML/CSS/JavaScript. No frameworks, no backend, no build step.

---

## 1. Run it locally

You just need any static file server (the app uses `fetch`/service workers, which
most browsers block on `file://`, so don't just double-click `index.html`).

**Option A — Python (already on most machines):**
```bash
cd birthday-pwa
python3 -m http.server 8080
```
Then open **http://localhost:8080** in Chrome.

**Option B — Node:**
```bash
npx serve .
```

**Option C — VS Code:** install the "Live Server" extension, right-click
`index.html` → "Open with Live Server".

---

## 2. Deploy on GitHub Pages

1. Create a new GitHub repository and push all these files to it (keep the
   folder structure as-is, `index.html` at the repo root or in `/docs`).
2. In the repo, go to **Settings → Pages**.
3. Under "Build and deployment", choose **Deploy from a branch**, pick
   `main` and the `/ (root)` folder.
4. Save. GitHub gives you a URL like:
   `https://yourname.github.io/repo-name/`
5. Open that link in Chrome on the phone — it works instantly, no install step.

---

## 3. Deploy on Netlify

1. Go to [netlify.com](https://www.netlify.com) → **Add new site → Deploy manually**.
2. Drag and drop the whole `birthday-pwa` folder onto the upload area.
3. Netlify gives you a live URL immediately (you can rename it in
   **Site settings → Change site name**).

---

## 4. Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) → **Add New → Project**.
2. Choose "Deploy without Git" / drag-and-drop the folder, or push the folder
   to a GitHub repo first and import it.
3. Framework preset: choose **Other** (it's static files, no build command
   needed).
4. Deploy — Vercel gives you a `https://your-project.vercel.app` link.

---

## 5. Add to Home Screen (for her)

Once she opens your link in Chrome on Android:
1. Tap the **⋮** menu in Chrome.
2. Tap **"Add to Home screen"**.
3. It now behaves like an installed app, with its own icon and no browser bar.

She does **not** need to do this — the link works perfectly fine on its own.

---

## 6. Customize it

### Change her name
Open `app.js`, find the `CONFIG` object near the very top:
```js
const CONFIG = {
  NAME: "",                 // put her name here, e.g. "Zara"
  SHOW_NAME_ON_FINAL: true,
  ...
};
```
Set `NAME` to her name. It will appear on the final card as
"Happy Birthday, Zara!". Leave it as `""` to keep things name-free.

### Change the birthday messages
Still in `app.js`:
- `BALLOON_WISHES` — the array of 30 short wishes used in the balloon game.
- `GALLERY_WISHES` — the array of 20 longer, warmer messages in the wish gallery.
- `GIFT_FIRST_WISH` — the single message revealed when the gift box is opened.
- The final card's paragraph text lives directly in `index.html` inside
  `#screen-final .final-text` if you want to tweak the closing message.

Just edit the strings — no other code changes needed.

### Change the music
The "soft instrumental piano" is not an audio file — it's generated live in the
browser with the Web Audio API (see the `AudioEngine` module in `app.js`,
function `scheduleMusicLoop`). This keeps the whole app tiny, offline-capable,
and free of licensing concerns.

To change its mood, edit the `SCALE` array (the notes it's allowed to play,
in Hz) or `stepDur` (the tempo) inside `AudioEngine`. If you'd rather use a
real audio file instead:
1. Add your `.mp3`/`.ogg` file to `assets/sounds/`.
2. Replace the call to `AudioEngine.startMusic()` in `app.js` with a simple
   `<audio>` element (e.g. `const music = new Audio('assets/sounds/music.mp3'); music.loop = true; music.play();`).
3. Add the file path to the `PRECACHE_URLS` list in `service-worker.js` so it
   still works offline.

### Change colors / fonts
All design tokens (colors, radii, fonts) are CSS custom properties at the top
of `style.css` under `:root`. Change a hex value once and it updates
everywhere.

### Change the icon
Re-run `gen_icons.py` (requires Python + Pillow: `pip install pillow`) after
editing the palette or shape inside it, or simply replace the PNG files in
`assets/icons/` with your own (keep the same filenames and sizes listed in
`manifest.json`).

---

## 7. File structure

```
birthday-pwa/
├── index.html          entry point, all screens/markup
├── style.css            design tokens + all styling
├── app.js               all app logic (config, content, audio, screens, games)
├── manifest.json         PWA manifest (name, icons, colors)
├── service-worker.js     offline caching
├── gen_icons.py          script that generated the icons (optional to keep)
├── assets/
│   └── icons/            all app icons (PNG, multiple sizes + maskable)
└── README.md              this file
```

There are no separate audio or Lottie asset files — music and sound effects
are generated at runtime with the Web Audio API, and all animation is done
with CSS keyframes, the Canvas API, and GSAP (loaded from a CDN, used for a
couple of small enhancements) to keep the app lightweight and fully
self-contained.

---

## 8. A note on honesty

The hidden developer mode (tap the cake 10 times on the final screen) shows
project info. It intentionally doesn't claim a specific number of hours were
spent building it — it just says it was made with care. If you personalize
the copy further, it's worth keeping that same spirit: a genuine "made this
for you" beats a fabricated backstory.

Happy building — and happy birthday to whoever this is for. 🎂
