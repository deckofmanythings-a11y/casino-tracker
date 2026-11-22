# Google Cloud setup for Casino Session Tracker (Drive storage)

This gets you an **OAuth Client ID** so the app can sign users in with Google and store each person's
data in their own Google Drive (`drive.file` scope). You only do this once.

When you finish Part 1, **send me the Client ID** (it looks like `1234567890-abc123.apps.googleusercontent.com`
— it is public, not a secret, so it's fine to paste). Then I'll wire up the app and we test it with you
as the first user. Part 2 (verification) runs in the background and doesn't block our testing.

---

## Part 1 — Create the project + Client ID  (do this now)

### 1. Create a project
1. Easiest: go straight to <https://console.cloud.google.com/projectcreate>, name it `casino-tracker`,
   click **Create**, then make sure it's selected.
2. (Or via the UI: in the **top bar**, click the **project dropdown** next to the "Google Cloud" logo
   — the one showing your current project name — then **New Project** at the top-right of that dialog.
   Note: this is the top-bar project picker, *not* the left-hand product/navigation menu.)

### 2. Enable the Drive API
1. Left menu → **APIs & Services → Library**.
2. Search **Google Drive API** → open it → **Enable**.

### 3. Configure the consent screen ("Google Auth Platform")
Google's new UI calls this **Google Auth Platform** (APIs & Services → OAuth consent screen now opens it).
If you see an Overview that says "not configured yet", click **Get started** and complete the wizard:
- **App Information:** App name `Casino Session Tracker`; User support email = your email.
- **Audience:** **External**.
- **Contact Information:** your email. → Agree → **Create**.

Then use the left-nav sections:
1. **Branding** →
   - **App home page:** `https://deckofmanythings-a11y.github.io/casino-tracker/`
   - **Privacy policy:** `https://deckofmanythings-a11y.github.io/casino-tracker/privacy.html`
   - **Authorized domains:** add `deckofmanythings-a11y.github.io` (the FULL subdomain — `github.io`
     alone is rejected as a public suffix). If Google demands Search Console verification of it, you can
     **leave this empty for now** — authorized domains + the URL fields are only required when you
     publish for verification (Part 2), not for Testing-mode use or for creating the Client ID.
   - App logo: optional (square PNG; makes the consent screen look trustworthy). → Save.
2. **Audience** → **Test users → Add users** → your own Google account. Leave status as **Testing** for now.
3. **Data Access** → **Add or remove scopes** → add these, then **Update → Save**:
   - `openid`
   - `.../auth/userinfo.email`
   - `.../auth/userinfo.profile`
   - **`.../auth/drive.file`**  ← the important one (per-file Drive access)
   - All four are **non-sensitive**, which keeps verification light (no security assessment).

### 4. Create the OAuth Client ID
1. **Google Auth Platform → Clients → Create client** (older UIs: APIs & Services → Credentials →
   Create credentials → OAuth client ID).
2. **Application type: Web application.** Name: `casino-tracker web`.
3. **Authorized JavaScript origins** — add each of these (exactly, no trailing slash):
   - `https://deckofmanythings-a11y.github.io`
   - `http://localhost:8080`
   - `http://127.0.0.1:8080`
4. **Authorized redirect URIs:** leave empty (the Google Identity popup doesn't need one).
5. Create → **copy the Client ID**.

### 5. Send me the Client ID
Paste it to me and I'll build the Drive integration. We'll test on `http://localhost:8080` and on the
live GitHub Pages URL with your account.

---

## Part 2 — Publish for everyone else  (do this once the app works for you)

While in **Testing**, only your listed test users can sign in, and sign-ins may need re-approval every
few days. To hand the app to anyone:

1. **Google Auth Platform → Audience → Publish app → Confirm** (moves it to "In production"). Verification
   is then tracked under **Verification Center**.
2. Google will ask you to **verify** the app. Because every scope we use is **non-sensitive**, this is
   the lighter **brand verification** (app name, logo, the home-page and privacy-policy URLs above, and
   the `github.io` authorized domain) — **not** the expensive restricted-scope security assessment.
3. Submit. Review is typically a few days to a couple of weeks. Until it's approved, outside users may
   see an "unverified app" warning they can click past, but publishing removes the test-user cap.

---

## Notes
- The **Client ID is public** and will live in the app's HTML — that's normal and safe for this flow.
  There is **no client secret** in this setup (browser apps don't use one).
- Before publishing, open `privacy.html` and fill in the **effective date** and a **contact email**
  (I left those as placeholders so your personal email isn't committed unless you choose to).
- No billing is required for any of this.
