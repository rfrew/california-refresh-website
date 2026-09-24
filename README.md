# California Refresh Cleaning Service

Marketing site for Yesenia G.'s home cleaning business in San Luis Obispo County.
Live at https://www.californiarefresh.com/ (English) and https://www.californiarefresh.com/es/ (Spanish).

## Structure
- `index.html` — English page (HTML/CSS/JS inlined)
- `es/index.html` — Spanish page. Its `<style>` block is identical to the English page's; keep the two in sync.
- `404.html`, `es/404.html` — "page not found" pages (English, and Spanish for anything under `/es/`)
- `assets/` — logos and favicon
- `robots.txt`, `sitemap.xml` — search engine files
- `wrangler.jsonc` — Cloudflare Workers config: static assets served from the repo root; unknown paths get the
  nearest `404.html` with a 404 status (`not_found_handling`)
- `.assetsignore` — repo files that must not be published (`.git`, this README, config files)

## Development
No build step. The pages use root-relative paths (`/assets/...`), so serve the folder instead of opening the file directly:

```sh
python3 -m http.server 8000   # then open http://localhost:8000/ and /es/
```

`npx wrangler dev` restarts in a loop here because the assets directory is the repo root (it watches its own
`.wrangler/` folder). To test Cloudflare behaviour such as the 404 pages, copy the site files into a `public/` folder in a scratch directory
with a copy of `wrangler.jsonc` pointing `assets.directory` at `./public`.

The redirect from `californiarefresh.com` to `www.californiarefresh.com` is a Cloudflare Redirect Rule in the
business's account (Rules → Redirect Rules), not part of this repo.

## Deployment
Hosted on Cloudflare Workers (static assets), Worker name `california-refresh-website`.
Every push to `main` deploys automatically:

- `.github/workflows/deploy.yml` (GitHub Actions) deploys to the business's Cloudflare account, using the
  repo secrets `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`. Re-run it from the Actions tab if needed.
- During the handoff, Workers Builds in Rob's Cloudflare account also deploys the same commit. That
  connection is removed once the domain has moved to the business's account.

The domain `californiarefresh.com` is registered with Cloudflare Registrar; DNS and Email Routing are on Cloudflare.

## Contact form
Both pages post to Formspree form `mzdworbp` (the business's Formspree account), which emails
`ca.refreshcleaning1@gmail.com`. If the form ID changes, update it in both `index.html` and `es/index.html`.
