# Casino Session Tracker — Discord Activity (frontend)

A private, single-user Discord **Activity** for logging real-money casino sessions live
from your phone: fast-select the game, enter the buy-in, track slot denomination + base
bet, log **bonus hits as a multiple of the base bet**, cash out, and watch a running
daily net — plus history, lifetime stats, W-2G jackpot flagging (≥ $1,200), and trip
bankroll tracking.

Static single-page app (no build step), hosted on GitHub Pages, embedded in Discord via
the Embedded App SDK. Backend: `../casino-tracker-functions` (Supabase Edge Functions in
the shared raided-hex project). Mirrors the raided-hex Activity stack.

## Files

- `index.html` — the whole app (UI + logic + a `?mock=1` offline demo engine).
- `sb-client.bundle.js` — vendored Supabase JS client. **Deliberately NOT named
  `supabase*`**: Discord Activity URL Mappings match path prefixes as plain strings, and a
  `/supabase` mapping would swallow a `/supabase-js*.js` request. (Casino-project gotcha.)
- `discord-embedded-app-sdk.js` — vendored Discord Embedded App SDK.

## Preview without Discord

Open `index.html?mock=1` in any browser (or the served URL) — it runs the full UI against
in-memory demo data with no backend. Nothing is saved.

## Configure before it works in Discord

In `index.html`, set `DISCORD_CLIENT_ID` to the tracker's Discord application client ID.
`SUPABASE_URL` / `SUPABASE_ANON_KEY` already point at the shared raided-hex project.

## Discord Developer Portal (URL Mappings)

- root `/` → `deckofmanythings-a11y.github.io/casino-tracker/`
- `/supabase` → `uaoxvhihwiygrajicend.supabase.co`

and add the Pages URL as an OAuth2 Redirect URI (required or `authorize()` throws).

## Versioning

`APP_VERSION` in `index.html`, shown bottom-right, bumped on every push.
