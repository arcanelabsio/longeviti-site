# longeviti-site

Marketing site for [Longeviti](https://longeviti.app) — a practitioner-led nutrition coaching platform. Currently serving an informational landing page while the client-facing platform is built out in 2026.

**Developed at [Arcane Labs](https://arcanelabs.info)** · contact: hello@arcanelabs.info
**Live at:** [longeviti.app](https://longeviti.app)
**Built with:** Jekyll (GitHub Pages' built-in generator — no Actions needed)
**License:** [Dual — site code MIT, content CC BY 4.0](./LICENSE). Trademarks reserved.

---

## Local development

You need Ruby 3.1+ and Bundler.

```bash
# One-time
bundle install

# Serve locally on http://localhost:4000
bundle exec jekyll serve --livereload
```

Files change → Jekyll rebuilds → browser reloads.

## Structure

```
_config.yml            ← Jekyll config
_layouts/
  default.html         ← Shell used by all pages
_includes/
  head.html            ← <head> contents
  nav.html             ← Top nav bar
  footer.html          ← Footer
index.html             ← Informational landing page (at /)
404.html               ← Not-found page
assets/
  css/main.css         ← Design tokens
  img/                 ← Static images
CNAME                  ← longeviti.app (for GitHub Pages custom domain)
Gemfile                ← Pinned to github-pages gem version
```

The `_docs/` collection and docs hub were removed during the 2026 rebuild — they described the old BYO-Claude architecture, which no longer matches what we're building. Docs will return under a new structure once the new client-facing platform is live.

## Editing content

### Change a design token

Everything visual comes from `assets/css/main.css` CSS variables in `:root`. These mirror `lib/theme/app_theme.dart` in the app repo — if you change a token here, update the app too (or vice versa) to keep the brand in sync.

## Deploying

GitHub Pages is enabled on `main` branch, serving from `/` (root). Every push to `main` triggers a rebuild on GitHub's servers (Jekyll mode). No Actions workflow needed.

### DNS setup (one-time, on Porkbun)

For apex `longeviti.app`, add four A records:

```
A   @   185.199.108.153
A   @   185.199.109.153
A   @   185.199.110.153
A   @   185.199.111.153
```

And a CNAME for `www`:

```
CNAME   www   arcanelabsio.github.io
```

In the GitHub repo: Settings → Pages → Custom domain → `longeviti.app` → Enforce HTTPS.

Propagation takes 5–30 minutes. `dig longeviti.app +short` from your terminal confirms when the A records resolve.

## Contributing

Fixes and clarifications welcome. Please:

- Keep the voice consistent with existing pages (direct, specific, low marketing-speak)
- Run `bundle exec jekyll build` locally before pushing to catch Liquid errors
- One topic per PR
