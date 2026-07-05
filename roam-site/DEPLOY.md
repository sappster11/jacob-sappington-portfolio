# Deploying the Roam site

This folder is the deployable site: four self-contained HTML pages (fonts
embedded, no build step, no dependencies). Links use clean URLs (`/apply`,
`/essay`, `/terms`), which both Cloudflare Pages and Vercel resolve to the
matching `.html` files automatically (`vercel.json` handles it on Vercel).

## Option A — Cloudflare Pages

1. Cloudflare dashboard → **Workers & Pages → Create → Pages →
   Connect to Git** and pick this repo.
2. Settings: production branch = your main branch, **build command: none**,
   **build output directory: `roam-site`**.
3. Deploy. You get `<project>.pages.dev` immediately.
4. Custom domain: add it under the project's **Custom domains** tab. If the
   domain's DNS is on Cloudflare it's one click.

## Option B — Vercel (recommended once the backend lands)

1. vercel.com → **Add New → Project**, import this repo.
2. Framework preset: **Other**. Root directory: `roam-site`. No build command.
3. Deploy. Custom domain under Settings → Domains.

Why B long-term: the backend is already on Vercel. Same project (or same
domain) means the forms post to `/api/...` with no CORS, one dashboard, and
per-branch preview URLs.

Either way, Cloudflare can stay in front as registrar/DNS/proxy — Cloudflare
for DNS + Vercel for hosting is a normal, happy combo.

## Wiring the forms to the backend (when ready)

All three forms currently degrade gracefully (mailto: or inline success).
Swap them to real endpoints by changing only the JS submit handlers:

- **Apply form** (`apply.html`, `#applyForm`): replace the `mailto:` handler
  with `fetch('/api/apply', {method:'POST', body: JSON.stringify(fields)})`,
  then show the success state.
- **Newsletter** (`index.html` `#newsForm`, `essay.html` `.js-news`):
  `fetch('/api/subscribe', ...)` or your ESP's form endpoint (Klaviyo/Loops
  both offer one). A retention studio's own list should live in a real ESP
  from day one.

## Housekeeping before real launch

- Replace placeholder client names/logos, stats, and case media.
- Have counsel review `terms.html` (the page itself says so).
- Point `hello@roam.studio` (or your real domain) at a real inbox.
- Add favicon + OG image (the italic serif "r" on ink).
