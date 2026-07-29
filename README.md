# trakolo-website

The public marketing site — eventually `trakolo.com` / `www.trakolo.com`. Static HTML/CSS, no build step, no framework. Hosted on **GitHub Pages**, deliberately separate from [`kjmutt/trakolo`](https://github.com/kjmutt/trakolo) (the product application) and [`kjmutt/trakolo-infra`](https://github.com/kjmutt/trakolo-infra) (cloud infrastructure) — see the reasoning below.

**Current state: previewing at `https://kjmutt.github.io/trakolo-website/`.** No custom domain wired up yet — no `CNAME` file, no DNS changes. See "Moving to the custom domain" below for that step when ready.

## Why this is its own repo

| | Product app (`trakolo`) | This site |
|---|---|---|
| Audience | Logged-in tenant users | Anonymous public visitors |
| Who edits it | Engineers | Marketing/content, occasionally engineers |
| Review bar | 1 approval + CI (unit tests, SAST) | Lighter — a homepage copy edit doesn't need a schema-change-grade gate |
| Deploy cadence | Tied to the app release train | Ships independently, far more often |
| Hosting | Azure (Container Apps + Static Web Apps) | GitHub Pages — free, no build step needed for static HTML |
| Domain | `{tenant}.trakolo.com`, `portal.trakolo.com` | `trakolo.com` / `www.trakolo.com` (once wired up) |

## Enabling the preview

Settings → Pages → **Source**: Deploy from a branch → `main` / `(root)` → Save. Live within a minute at `https://kjmutt.github.io/trakolo-website/`.

**Requires the repo to be public** — GitHub Pages on private repos needs a paid GitHub plan.

## Moving to the custom domain (later)

1. Add a `CNAME` file at the repo root containing `trakolo.com`.
2. Add these DNS records at the registrar:

| Type | Host | Value |
|---|---|---|
| A | `@` (apex `trakolo.com`) | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `kjmutt.github.io` |

3. Settings → Pages → **Custom domain** → enter `trakolo.com` → wait for the DNS check to pass → enable **Enforce HTTPS** (certificate issuance can take up to 24 hours after DNS propagates).

## How this site links to the actual application

There's no shared runtime or shared codebase — just a few links crossing the domain boundary:

- **Sign in / Start free** buttons → `https://portal.trakolo.com/login`, the real tenant-agnostic sign-in chooser served by the product app.
- **"See it in action" links** (e.g. "See TS-4833 on the desk →") → `https://demo.trakolo.com/...`, a public, sandboxed demo tenant — never the authenticated product domain.
- **Signup** writes to the real tenant registry, so it's a product-side concern: this site's calls-to-action link out to the product's sign-up flow rather than re-implementing tenant provisioning here.
- **Contact** (`contact.html`) is a real page in this repo, but its form doesn't post anywhere yet — wire it to a form-handling service (or a lightweight serverless function, e.g. an Azure Function or a GitHub Pages-compatible form service like Formspree) before launch, since GitHub Pages itself only serves static files and can't run backend form-handling code.

## Pages

`index`, `how-it-works`, `features`, `use-cases`, `customer-stories`, `integrations`, `pricing`, `docs`, `investors`, `release-notes`, `contact` — all static, all sharing `styles.css` and `nav.js`.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the (lighter) branching and review process.
