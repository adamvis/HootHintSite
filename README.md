# HootHintSite

Public-facing half of HootHint: a static site (plain HTML/CSS, no build
step) plus this repo's GitHub Releases, which host the actual downloadable
installer.

The app's source lives in a separate, **private** repo. Nothing here is
source code — this repo exists so there's something public to point
strangers at: a download link, setup instructions, an FAQ, and a privacy
page, without exposing the source (in particular, the shared Telegram bot
token baked into it — see the source repo's CLAUDE.md).

## Structure

- `index.html`, `getting-started.html`, `faq.html`, `privacy.html`,
  `changelog.html` — the site, all sharing `style.css`. No templating —
  each page repeats its own header/nav/footer, matching the rest of this
  project's "no build step" philosophy.
- `assets/` — resized copies of the app's owl logo (`logo-512.png`,
  `logo-192.png`, `favicon-32.png`); the source-repo original is ~4.4MB and
  too large to serve directly on a landing page.
- `LICENSE.txt` — copy of the source repo's freeware license, linked from
  every page's footer.

## Publishing the site (GitHub Pages)

One-time setup: repo Settings → Pages → Source: **Deploy from a branch** →
Branch: `main`, folder: `/ (root)`. After that, every push to `main`
redeploys automatically — no CI config needed for a plain static site.

## Releasing a new version

The download links across the site (`index.html`'s hero/nav button,
`getting-started.html`, `faq.html`, `changelog.html`) all point at:

```
https://github.com/adamvis/HootHintSite/releases/latest/download/HootHint-Setup.exe
```

GitHub's `/releases/latest/download/<asset-name>` URL always redirects to
whichever release is currently marked "Latest" — so the links never need
to change per version, as long as the uploaded asset is named exactly
`HootHint-Setup.exe` every time.

To ship a new version:

1. In the source repo, bump `theme.VERSION` and `installer.iss`'s
   `MyAppVersion` together, then run `build.bat` to produce a signed
   `..\screen-snapshot-build\installer\HootHint-Setup.exe`.
2. Here, create a new GitHub Release (tag `vX.Y.Z`), upload that `.exe` as
   its asset (keep the filename `HootHint-Setup.exe`), and add release
   notes.
3. Add a matching entry to `changelog.html`.
