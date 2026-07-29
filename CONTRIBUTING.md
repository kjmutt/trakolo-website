# Branching strategy

Trunk-based, same shape as the other two repos, deliberately lighter:

- `main` is always deployable and is the only long-lived branch.
- `content/<short-desc>` for copy/page changes, `fix/<short-desc>` for bugs. Short-lived, cut from `main`.
- No feature-flag system here — a static site has nothing to flag; unfinished pages just don't get linked from nav until ready.

## Pull request requirements

Lighter than the application repo on purpose — a pricing-copy fix shouldn't need the same gate as a schema migration:

1. **CI passing** — HTML validity + a broken-link check across the site.
2. **One approving review.**
3. **No direct pushes to `main`.**

See [`.github/rulesets/main-branch-ruleset.json`](.github/rulesets/main-branch-ruleset.json) — import via Settings → Rules (see that file's header comment).

## Cross-domain links

Any link to `portal.trakolo.com` or `demo.trakolo.com` is intentional and should stay an absolute URL — never rewrite these to relative paths, this site is not deployed alongside the product app.
