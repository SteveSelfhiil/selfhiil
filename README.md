# selfhiil.com

Static copy of https://selfhiil.com (Vietnamese + English), packaged as a Cloudflare Worker with static assets.

```
public/                  the whole site (served as-is)
  index.html             home (vi)
  ve-self-hiil/ giai-phap-hiilee-6pr/ chuong-trinh/ sach-hiilee-6pr/ lien-he/
  blog/                  blog index + 11 articles
  en/                    English: about/ hiilee-6pr/ programs/ book/ blog/ contact/
  assets/css/style.css
  assets/selfhiil-logo.svg
wrangler.toml            Worker config (assets only, no script)
package.json             wrangler dev / deploy
```

## How this copy was made (2026-10-03)

- All 25 pages, the stylesheet and the logo were downloaded byte-for-byte from the live site.
- Cloudflare had rewritten the email links (`/cdn-cgi/l/email-protection` + `email-decode.min.js`).
  Those were turned back into plain `mailto:hiila@selfhiil.com` links and the injected script was removed.
  If Email Address Obfuscation stays on in Cloudflare, it will re-apply automatically.
- `robots.txt` was not copied: it was Cloudflare's managed robots.txt, which Cloudflare keeps serving on its own.
- Google Fonts (Montserrat) still load from fonts.googleapis.com, as on the live site.

## Deploy on Cloudflare (Workers Builds, connected to GitHub)

1. Cloudflare dashboard > Workers & Pages > Create > Import a repository > `SteveSelfhiil/selfhiil`.
2. Project name: `selfhiil` (must match `name` in wrangler.toml).
3. Build command: leave empty. Deploy command: `npx wrangler deploy`. Root directory: `/`.
4. Deploy, then check the `*.workers.dev` URL.
5. Point the domain at the Worker: DNS > remove the old `selfhiil.com` A/AAAA record (the nginx server),
   then Worker > Settings > Domains & Routes > Add > Custom domain > `selfhiil.com`.

Every push to `main` redeploys automatically.

## Local preview / manual deploy

```
npm install
npx wrangler dev      # http://localhost:8787
npx wrangler deploy   # manual deploy (needs `npx wrangler login`)
```
