# Harborage Marina Lease Extension Referendum

A single-page informational website for the St. Petersburg citywide referendum on the
Harborage Marina lease extension (on the ballot November 3, 2026).

- **Framework:** [Astro](https://astro.build/) (static output)
- **Hosting:** Cloudflare Pages
- **Domain:** harboragereferendum2026.com (registered at GoDaddy, DNS managed by Cloudflare)

## Local development

```bash
npm install
npm run dev
```

The dev server runs at http://localhost:4321.

## Build

```bash
npm run build      # outputs static site to ./dist
npm run preview    # preview the production build locally
```

## Project structure

```
/
├── public/              # static assets served as-is (favicon, etc.)
├── src/
│   ├── layouts/
│   │   └── Layout.astro  # base HTML shell
│   ├── pages/
│   │   └── index.astro   # the single page
│   └── styles/
│       └── global.css    # colors and layout
└── astro.config.mjs
```

## Colors

| Purpose               | Hex       |
| --------------------- | --------- |
| Background            | `#223658` |
| Highlights / links    | `#FFFF00` |
| Body copy             | `#FFFFFF` |

## Placeholders to update

The following footer/contact values are placeholders. Replace them with real values:

- Contact email (`src/pages/index.astro` footer): `info@harboragereferendum2026.com`
- "View Official Election Information" button links to `votepinellas.gov` — confirm the correct URL.
- Political advertisement disclaimer in the footer, if legally required.

---

## Deployment: GoDaddy domain → Cloudflare Pages

The site auto-deploys to Cloudflare Pages on every push to the `main` branch on GitHub.
See below for the complete first-time setup, including pointing the GoDaddy domain to
Cloudflare.

### Part 1 — Connect the GitHub repo to Cloudflare Pages

1. Sign in at https://dash.cloudflare.com (create a free account if needed).
2. In the left sidebar go to **Workers & Pages** → **Create** → **Pages** tab → **Connect to Git**.
3. Authorize Cloudflare to access your GitHub account and select the
   `harboragereferendum2026` repository.
4. Configure the build settings:
   - **Production branch:** `main`
   - **Framework preset:** Astro
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
5. Click **Save and Deploy**. Cloudflare builds the site and serves it at a
   `*.pages.dev` URL. Confirm that preview URL works before continuing.

### Part 2 — Add the custom domain in Cloudflare Pages

1. Open your Pages project → **Custom domains** tab → **Set up a custom domain**.
2. Enter `harboragereferendum2026.com` and follow the prompts.
3. Also add `www.harboragereferendum2026.com` so both work.

Cloudflare will tell you the domain needs to use Cloudflare nameservers (Part 3).

### Part 3 — Move DNS from GoDaddy to Cloudflare (recommended)

Using Cloudflare's nameservers gives Pages automatic DNS + SSL for the apex domain.

1. In the Cloudflare dashboard, go to **Websites** → **Add a site**, enter
   `harboragereferendum2026.com`, and pick the **Free** plan.
2. Cloudflare scans existing DNS records. Review them, then it will show you **two
   assigned Cloudflare nameservers**, e.g.:
   ```
   xxxx.ns.cloudflare.com
   yyyy.ns.cloudflare.com
   ```
   Copy both.
3. Sign in to **GoDaddy** → **My Products** → find the domain → **DNS** →
   **Nameservers** → **Change** → **Enter my own nameservers (advanced)**.
4. Remove GoDaddy's default nameservers and enter the two Cloudflare nameservers.
   Save.
5. Back in Cloudflare, click **Done, check nameservers**. Propagation usually takes
   from a few minutes up to 24-48 hours.
6. Once Cloudflare shows the site as **Active**, return to your **Pages project →
   Custom domains**. Cloudflare automatically creates the required DNS records
   (CNAME/A) and provisions a free SSL certificate. Status should move to **Active**.

### Part 4 — Verify

- Visit `https://harboragereferendum2026.com` and `https://www.harboragereferendum2026.com`.
- Confirm the padlock (valid SSL) appears.
- In Cloudflare **SSL/TLS** settings, set encryption mode to **Full** (or **Full
  (strict)**).
- Optional: under **SSL/TLS → Edge Certificates**, enable **Always Use HTTPS**.

### Alternative — keep DNS at GoDaddy (CNAME only)

If you prefer not to move nameservers, you can instead add a `CNAME` record at GoDaddy
pointing to your `*.pages.dev` hostname. Note: the apex/root domain (`harboragereferendum2026.com`
without `www`) cannot use a plain CNAME at GoDaddy, so moving nameservers to Cloudflare
(Part 3) is the recommended path for a clean apex + www setup.

### Making changes later

Edit the content in `src/pages/index.astro`, commit, and push to `main`. Cloudflare
Pages rebuilds and redeploys automatically.
