# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Tech Stack

React 19 + Vite 6 + React Router 7 + Chart.js. Plain JavaScript (JSX, not TypeScript). Package manager: npm.

## Commands

- `npm run dev` — start dev server (Vite, default port 5173)
- `npm run build` — production build to `dist/`
- `npm run lint` — ESLint (the only quality gate; no Prettier)
- `npm run preview` — serve the production build locally

No test script is configured.

## Code Style

- Plain JSX (`.jsx`), not TypeScript — do not add `.ts`/`.tsx` extensions
- No formatter is configured; ESLint (`eslint .`) is the only quality gate
- ESLint uses flat config (`eslint.config.js`) with `react-hooks` and `react-refresh` plugins
- `no-unused-vars` is an error; variables matching `/^[A-Z_]/` are exempt
- All imports use relative paths — no path aliases are configured in Vite

## Environment Variables

`.env` is committed to the repo. Both vars point to the public CoinGecko API (no API key needed):

```
VITE_COINS_API_URL  # coin list endpoint
VITE_COIN_API_URL   # single coin detail endpoint
```

The `VITE_` prefix is required by Vite for env vars to be accessible via `import.meta.env`.

## API Gotcha

CoinGecko's free tier has a rate limit (~30 requests/min). Avoid triggering repeated API calls in quick succession during development — the app may return errors or empty state if the limit is hit.

## Project Structure

```
src/
  main.jsx          # entry — StrictMode + BrowserRouter
  App.jsx           # root — fetches coin list, owns routing
  components/       # shared UI components
  pages/            # home, about, coin-details, not-found
```

Routes: `/` (home), `/about`, `/coin/:id` (detail), `*` (404).

## PWA

The app is a fully installable Progressive Web App via `vite-plugin-pwa` (devDependency).

**How it works:**
- `vite.config.js` configures `VitePWA` with `registerType: 'autoUpdate'` — the service worker registers and updates silently, no user prompt needed.
- `npm run build` generates `dist/sw.js` (Workbox service worker) and `dist/manifest.webmanifest` automatically.
- The manifest link is injected into `index.html` by the plugin at build time; no manual `<link rel="manifest">` tag is needed.

**Caching strategy:**
- App shell (all JS, CSS, HTML, images, fonts in `dist/`) → precached by Workbox at build time, available offline.
- `api.coingecko.com` requests → `NetworkOnly` — live prices are never served from cache.

**Icons (`public/`):**
- Source: `icon.svg` (dark bg `#0e1117`, blue `#58a6ff` coin, green `#16a34a` chart line)
- Generated with `npx @vite-pwa/assets-generator --preset minimal public/icon.svg`: `pwa-64x64.png`, `pwa-192x192.png`, `pwa-512x512.png`, `maskable-icon-512x512.png`, `apple-touch-icon-180x180.png`, `favicon.ico`
- To regenerate icons after editing `icon.svg`, re-run the command above.

**Testing PWA install locally:**
```bash
npm run build && npm run preview
```
Open `http://localhost:4173` in Chrome/Edge — install icon appears in the address bar. DevTools → Application → Service Workers / Manifest to inspect.

**Deployed (Cloudflare):** HTTPS is provided automatically; push to `main` and visit the deployed URL to get the "Add to Home Screen" prompt on mobile or the install icon on desktop.
