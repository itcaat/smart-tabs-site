# Smart Tabs — Hugo landing

Single-page site for the **Smart Tabs** browser extension: hero, features, widgets with code samples (Chroma syntax highlighting), trust block, CTA. **English only.** Static output, ready for GitHub Pages or any static host.

## Requirements

- [Hugo Extended](https://gohugo.io/installation/) (for SCSS in `assets/scss/`). Check: `hugo version` should include `extended`.

## Local development

```bash
git clone <url> && cd smart-tabs-site
hugo server -D --baseURL http://localhost:1313/
```

`baseURL` in `hugo.toml` matches the GitHub Pages path (`/smart-tabs-site/`). Without `--baseURL` above, the dev server mirrors that path; with it, pages are at the site root (e.g. `/donate/`).

Site: [http://localhost:1313/](http://localhost:1313/) (with the flag) or [http://localhost:1313/smart-tabs-site/](http://localhost:1313/smart-tabs-site/) (default).

## Build

```bash
hugo --gc --minify
```

Output: **`public/`**

## Editing

| What | Where |
|------|--------|
| Page copy (hero, features, widgets, trust, footer) | [`content/_index.md`](content/_index.md) (YAML front matter) |
| `baseURL`, store link, privacy, meta description | [`hugo.toml`](hugo.toml) → `[params]` |
| Page layout | [`layouts/index.html`](layouts/index.html) |
| Widget card (form + code block) | [`layouts/partials/widget-editor.html`](layouts/partials/widget-editor.html) |
| Styles | [`assets/scss/main.scss`](assets/scss/main.scss) |
| Code highlight theme | [`hugo.toml`](hugo.toml) → `[markup.highlight]` and [`assets/css/chroma-github-dark.css`](assets/css/chroma-github-dark.css) (regenerate: `hugo gen chromastyles --style=github-dark > assets/css/chroma-github-dark.css`) |
| Images | `hero.preview` in `_index.md` and files under **`static/images/`** |

## GitHub Pages

Enable **Settings → Pages → Source: GitHub Actions**. Workflow [`.github/workflows/hugo-pages.yml`](.github/workflows/hugo-pages.yml) builds on push to `main` or `master`. CI passes the correct `--baseURL` for Pages.

## Layout (short)

```
content/_index.md    # homepage content
layouts/             # templates
assets/scss/         # styles
assets/css/          # Chroma CSS
static/              # static assets
public/              # build output (.gitignored)
```
