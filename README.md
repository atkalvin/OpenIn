# OpenIn POC – Smart Deep Link Landing

This is a **single-file** proof-of-concept to replicate what services like TapIt do:
- You host a page (`open.html`).
- Creators put **your** link in their bio instead of the direct YouTube/Twitch URL.
- When someone taps the link inside Instagram/TikTok in-app browsers, your page immediately **tries to open the app**, and falls back to the web URL if it fails.

## How to deploy (no backend needed)
1. Upload `open.html` to any static hosting (Netlify, Vercel, GitHub Pages, Cloudflare Pages).
2. Your link format:
   ```
   https://YOUR_DOMAIN/open.html?app=<URL-ENCODED_APP_SCHEME>&web=<URL-ENCODED_WEB_URL>&pkg=<ANDROID_PACKAGE_OPTIONAL>
   ```
3. Replace parameters:
   - `app`: the app deep-link scheme to try (e.g. `youtube://channel/UCxxxxxxxx`)
   - `web`: the web fallback (e.g. `https://www.youtube.com/channel/UCxxxxxxxx`)
   - `pkg` (optional): Android package to build an `intent://` link (e.g. `com.google.android.youtube`)

> Encode parameters! For example use `encodeURIComponent` or any URL encoder.

## Examples

### YouTube channel
```
https://YOUR_DOMAIN/open.html?app=youtube%3A%2F%2Fchannel%2FUCxxxxxxxx&web=https%3A%2F%2Fwww.youtube.com%2Fchannel%2FUCxxxxxxxx&pkg=com.google.android.youtube
```

### Generic (any app) without Android package
```
https://YOUR_DOMAIN/open.html?app=spotify%3A%2F%2Fartist%2F<ARTIST_ID>&web=https%3A%2F%2Fopen.spotify.com%2Fartist%2F<ARTIST_ID>
```

If you provide `pkg` on Android (e.g. `com.spotify.music`), the page will build an `intent://` link which often provides a more reliable experience.

## Notes & limits
- On iOS inside Instagram/TikTok, users will see the system prompt: **“Instagram wants to open ‘YouTube’”** — you cannot bypass it.
- If the app is not installed, the page falls back to the `web` URL automatically.
- This single file is intentionally simple for a POC. A production SaaS should add:
  - a link shortener (`/r/:slug`), a dashboard, templates for common apps,
  - analytics (clicks, app-open vs web-open),
  - rate limiting, theming, and custom domains.

Enjoy!
