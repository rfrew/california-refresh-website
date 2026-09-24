# California Refresh Cleaning Service

Marketing site for Yesenia G.'s home cleaning business in San Luis Obispo County.
Live at https://www.californiarefresh.com/ (English) and https://www.californiarefresh.com/es/ (Spanish).

## Structure
- `index.html` — English page (HTML/CSS/JS inlined)
- `es/index.html` — Spanish page. Its `<style>` block is identical to the English page's; keep the two in sync.
- `assets/` — logos and favicon
- `robots.txt`, `sitemap.xml` — search engine files
- `wrangler.jsonc` — Cloudflare Workers config (static assets, served from the repo root)
- `.assetsignore` — repo files that must not be published (`.git`, this README, config files)

## Development
No build step. The pages use root-relative paths (`/assets/...`), so serve the folder instead of opening the file directly:

```sh
python3 -m http.server 8000   # then open http://localhost:8000/ and /es/
```

## Deployment
Hosted on Cloudflare Workers (static assets), Worker name `california-refresh-website`.
Workers Builds is connected to this repo: every push to `main` deploys to production in about a minute.
Check a deploy in the commit's GitHub checks ("Workers Builds") or in the Cloudflare dashboard.

The domain `californiarefresh.com` is registered with Cloudflare Registrar; DNS and Email Routing are on Cloudflare.

## Contact form
Both pages post to Formspree form `mzdworbp` (the business's Formspree account), which emails
`ca.refreshcleaning1@gmail.com`. If the form ID changes, update it in both `index.html` and `es/index.html`.
