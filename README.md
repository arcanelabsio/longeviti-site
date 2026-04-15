# longeviti-site

Public documentation site for [Longeviti](https://longeviti.app) — the personal longevity tracking app and framework.

**Live at:** [longeviti.app](https://longeviti.app)
**Built with:** Jekyll (GitHub Pages' built-in generator — no Actions needed)
**License:** Content CC BY 4.0; code MIT

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
_config.yml            ← Jekyll config + nav + docs sidebar order
_layouts/
  default.html         ← Shell used by marketing pages (landing, docs hub)
  doc.html             ← Shell used by every doc page (adds sidebar)
_includes/
  head.html            ← <head> contents
  nav.html             ← Top nav bar
  footer.html          ← Footer
  docs-sidebar.html    ← Left sidebar on doc pages (reads site.docs_nav)
_docs/                 ← Doc pages as Markdown (collection, permalink /docs/:path/)
docs/
  index.html           ← Docs hub page (at /docs/)
index.html             ← Landing page (at /)
assets/
  css/main.css         ← Design tokens mirrored from the Flutter app's theme
  img/                 ← Static images
CNAME                  ← longeviti.app (for GitHub Pages custom domain)
Gemfile                ← Pinned to github-pages gem version
```

## Editing content

### Add a doc page

1. Create `_docs/my-new-page.md`:
   ```markdown
   ---
   title: My new page
   lead: One-line description used as subtitle.
   ---

   Page content in Markdown.
   ```
2. Add it to the sidebar by editing `_config.yml` → `docs_nav`.
3. Commit and push.

### Edit the sidebar order

Only in `_config.yml` under `docs_nav`. It's the single source of truth for nav structure.

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
CNAME   www   ajitgunturi.github.io
```

In the GitHub repo: Settings → Pages → Custom domain → `longeviti.app` → Enforce HTTPS.

Propagation takes 5–30 minutes. `dig longeviti.app +short` from your terminal confirms when the A records resolve.

## Contributing

Fixes and clarifications welcome. Please:

- Keep the voice consistent with existing pages (direct, specific, low marketing-speak)
- Run `bundle exec jekyll build` locally before pushing to catch Liquid errors
- One topic per PR
