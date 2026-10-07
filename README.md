# Find A Doctor — Cloudflare Live Queue V12

This package keeps Supabase as the backend/database/auth source of truth and uses Cloudflare Durable Objects + WebSockets only for live queue delivery.

## Important fixes
- Home doctor cards cache their complete doctor row, so opening a profile does not wait on a second Supabase query.
- If card data is unavailable, the fallback Supabase lookup has a 5-second timeout; the profile can never remain on an endless spinner.
- QR/public doctor queue pages no longer use 700ms polling.
- Cloudflare broadcast sends the current queue state to the Durable Object, which fans it out to connected WebSocket clients.

## Before production
1. Deploy `worker.js` with `wrangler deploy`.
2. Set `SUPABASE_URL` and `SUPABASE_ANON_KEY` as Worker variables/secrets.
3. Put the deployed Worker URL in `index.html` at `window.DQ_LIVE_WORKER_URL`.
4. Deploy the website after that change.

Do not remove `wrangler.toml` or `worker.js`; they are required for the Cloudflare live layer.
