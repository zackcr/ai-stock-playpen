# AI Stock Playpen

A personal buy-the-dip tracker: a watchlist of stocks with 52-week high/low gauges,
position tracking (including per-lot cost basis), and optional live pricing, portfolio
guardrails, and one-way Google Sheet sync. Runs entirely client-side as an installable
PWA — no backend, no build step required to run it.

## Files

- `index.html`, `app.js`, `sw.js`, `manifest.json`, `icon-192.png`, `icon-512.png` —
  the deployed app. `app.js` is a pre-built React bundle; this is what `index.html`
  actually loads and what you deploy as-is (e.g. to GitHub Pages).
- `ai-stock-playpen.jsx.txt` — the editable React source for `app.js`. Edit this file,
  then rebundle it into `app.js` (e.g. with esbuild: bundle it together with React/
  ReactDOM as globals and write the result to `app.js`). Don't edit `app.js` by hand —
  it won't stay in sync with this file.
- `ai-stock-playpen-pwa.zip` — a packaged copy of the six deployed files above, for
  distributing the app as a download. Rebuild it whenever `app.js` changes.

## Setup

- **Live pricing**: get a free API key at finnhub.io/register (60 requests/min, no
  card required) and paste it into in-app Settings. Without a key, prices are entered
  manually and live-data features (auto-fill on add, day range/market cap/P-E/etc.,
  news/earnings/analyst view, notifications) are unavailable.
- **Trend context**: the trend pill is a proxy built from Finnhub's 13-week/26-week
  price return figures — a true 50/200-day SMA isn't available for free (Finnhub's
  historical daily candles are paywalled, and Stooq turned out to be unreliable through
  public CORS proxies).
- **Google Sheet sync** (optional): in Settings, paste the URL of an Apps Script web
  app you deploy yourself; the in-app template (Extensions → Apps Script in your
  sheet) is provided in the Settings panel. It's a one-way push — it overwrites the
  sheet each time and never reads anything back.
- **Backup**: Settings also has a JSON export/import (full state, including the API
  key in plain text — treat the export file like a password) and a CSV export.

All state (tickers, positions, settings, price history) lives in the browser's
`localStorage` — nothing is sent anywhere except the API calls described above.
