# trakolo-website

The public marketing site — `trakolo.com` / `www.trakolo.com`. Static HTML/CSS, no build step, no framework. This is a deliberately separate repo from [`kjmutt/trakolo`](https://github.com/kjmutt/trakolo) (the product application) and [`kjmutt/trakolo-infra`](https://github.com/kjmutt/trakolo-infra) (cloud infrastructure) — see the reasoning below.

## Why this is its own repo

| | Product app (`trakolo`) | This site |
|---|---|---|
| Audience | Logged-in tenant users | Anonymous public visitors |
| Who edits it | Engineers | Marketing/content, occasionally engineers |
| Review bar | 1 approval + CI (unit tests, SAST) | Lighter — a homepage copy edit doesn't need a schema-change-grade gate |
| Deploy cadence | Tied to the app release train | Ships independently, far more often |
| Domain | `{tenant}.trakolo.com`, `portal.trakolo.com` | `trakolo.com` / `www.trakolo.com` |

Same Azure Front Door resource routes both — different host/path rules, not two separate edge layers. See the application repo's architecture documentation for the full edge/routing picture.

## How this site links to the actual application

There's no shared runtime or shared codebase — just a few links crossing the domain boundary:

- **Sign in / Start free** buttons → `https://portal.trakolo.com/login`, the real tenant-agnostic sign-in chooser served by the product app.
- **"See it in action" links** (e.g. "See TS-4833 on the desk →") → `https://demo.trakolo.com/...`, a public, sandboxed demo tenant — never the authenticated product domain.
- **Signup** writes to the real tenant registry, so it's a product-side concern: this site's calls-to-action link out to the product's sign-up flow rather than re-implementing tenant provisioning here.
- **Contact** (`contact.html`) is a real page in this repo, but its form doesn't post anywhere yet — wire it to a form-handling service (or a lightweight serverless function) before launch. It is not connected to the product API.

## Pages

`index`, `how-it-works`, `features`, `use-cases`, `customer-stories`, `integrations`, `pricing`, `docs`, `investors`, `release-notes`, `contact` — all static, all sharing `styles.css` and `nav.js`.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the (lighter) branching and review process.
