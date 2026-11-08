# www.taku.ie

Static landing page for <https://www.taku.ie>, hosted on Cloudflare Pages.
No build step: everything served lives in `public/`.

## Layout

- `public/index.html`, `public/404.html`, `public/style.css` – the site
- `public/_headers` – security headers (Cloudflare Pages format)
- `public/robots.txt`
- `wrangler.jsonc` – Cloudflare config (static assets from `public/`, serves `404.html` for unknown paths)

## Local preview

```bash
python3 -m http.server -d public 8000
```

## Deploy (Cloudflare Pages, git integration)

1. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to git → select this repo.
2. Build settings: build command *(empty)*; deploy command `npx wrangler deploy` (reads `wrangler.jsonc`, which points at `public/`).
3. Production branch: `main`. Other branches/PRs get preview URLs automatically.
4. Project → Custom domains → add `www.taku.ie` (and `taku.ie`; see below).

### DNS cutover from the VM

- If `taku.ie` is on Cloudflare DNS, adding the custom domain creates the CNAME for you; delete the old A/AAAA record for `www` first.
- Redirect the apex to www: Rules → Redirect Rules → `taku.ie/*` → `https://www.taku.ie/${1}` (301).
- Keep the VM vhost until the Pages site is verified, then decommission it.
