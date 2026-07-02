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
