# Casino Session Tracker — web app

A private, mobile-friendly **web app** for logging real-money casino sessions: fast-select
the game, enter buy-ins (cents-first for machines), track slot denomination + base bet, log
**bonus hits as a multiple of the base bet**, scan TITO tickets to auto-fill cash-outs, watch
a live daily net and trip "in pocket", and review History, lifetime Stats, and a biggest-bonus
leaderboard.

Static single-page `index.html` (no build step) on GitHub Pages, backed by Supabase.
**Sign in with email + password (Supabase Auth).** Add it to your phone's home screen (PWA)
for an app-like experience. Companion backend repo: `../casino-tracker-functions`.

> Previously a Discord Activity — retired in favor of a standalone site (email/password,
> reliable session persistence, open multi-user sign-ups). The `git` history has that version.

## Files

- `index.html` — the whole app (auth screens + UI + logic + a `?mock=1` offline demo engine).
- `sb-client.bundle.js` — vendored Supabase JS client (auth + edge-function calls).
- `manifest.webmanifest` — PWA manifest for "Add to Home Screen".

## How it works

- **Auth:** Supabase Auth email/password. The client holds the session (persisted +
  auto-refreshed by supabase-js) and sends the user's JWT to the edge functions, which scope
  all data to that account (`requireUser`). Nothing in the `ct_` tables is publicly readable.
- **Preview without a login:** open `index.html?mock=1` — full UI against in-memory demo data,
  nothing saved.

## Supabase Auth setup (dashboard)

- **Authentication → Providers → Email:** enabled. Confirmation currently off (signups log in
  immediately); enable it later with SMTP if you want verified emails.
- **Authentication → URL Configuration:** add the site URL
  `https://deckofmanythings-a11y.github.io/casino-tracker/` to **Site URL** and the
  **Redirect URLs** allow-list, so password-reset links return to the app.

## Versioning

`APP_VERSION` in `index.html`, shown bottom-right, bumped on every push.
