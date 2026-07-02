---
name: verify
description: Lint the codebase and smoke-test the running app in a browser. Use after making changes to confirm nothing is broken.
---

Run `npm run lint` and report any errors or warnings.

Then check if the dev server is already running (port 5173 or 5174). If not, start it with `npm run dev` in the background and wait for it to be ready.

Once the server is up, use a browser or curl to verify the following routes:
1. `/` (home) — loads and displays a list of coins
2. `/coin/:id` for a known coin (e.g., `/coin/bitcoin`) — shows coin detail and chart
3. An unknown route (e.g., `/unknown`) — shows the 404 page

Report what passed, what failed, and any console errors or network errors encountered. Note any CoinGecko rate-limit errors (HTTP 429) separately — these are transient and not a code bug.
