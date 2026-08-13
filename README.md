# Ashir's Website (Hugo + Blowfish)

Personal blog and portfolio site, built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme. Hosted for free on GitHub Pages.

## Project structure

```
AshirWebsiteHugo/
├── .github/workflows/hugo.yml   # CI: builds and deploys the site to GitHub Pages on push to main
├── archetypes/                  # Front-matter templates used by `hugo new content ...`
│   ├── posts.md
│   └── projects.md
├── assets/                      # Files processed by Hugo Pipes (custom SCSS/JS overrides, images to be fingerprinted)
├── config/
│   └── _default/
│       ├── hugo.toml            # Core site config (baseURL, taxonomies, outputs, etc.)
│       ├── params.toml          # Theme/behavior options (Blowfish-specific)
│       ├── languages.en.toml    # Site title, description, author bio/social links
│       └── menus.en.toml        # Header navigation menu
├── content/                     # All site content (Markdown)
│   ├── _index.md                # Homepage content
│   ├── posts/                   # Blog posts
│   ├── projects/                # Portfolio / experience write-ups
│   └── about/                   # About page
├── data/                        # Structured data files (YAML/JSON/TOML) available to templates
├── i18n/                        # UI string translations (only needed for multi-language sites)
├── layouts/                     # Custom layout/partial overrides (leave empty to use theme defaults)
├── static/                      # Files copied as-is to the site root (favicon, images, CNAME, etc.)
├── themes/blowfish/             # Blowfish theme (git submodule, do not edit directly)
├── .gitignore
└── README.md
```

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (this project was scaffolded with v0.164.0)
- Git

## Local development

```powershell
# Clone with the theme submodule
git clone --recurse-submodules <your-repo-url>
cd AshirWebsiteHugo

# If you already cloned without submodules:
git submodule update --init --recursive

# Run the local dev server (with drafts visible)
hugo server --buildDrafts --buildFuture
```

Site will be available at `http://localhost:1313`.

## Adding content

```powershell
# New blog post
hugo new content posts/my-new-post.md

# New project write-up
hugo new content projects/my-new-project.md
```

Set `draft: false` in the front matter when ready to publish.

## Updating the theme

The theme is pinned as a git submodule. To update to the latest Blowfish release:

```powershell
cd themes/blowfish
git fetch --tags
git checkout <latest-tag>
cd ../..
git add themes/blowfish
git commit -m "Update Blowfish theme"
```

## Deployment

Deployment is automated via [`.github/workflows/hugo.yml`](.github/workflows/hugo.yml): every push to `main` builds the site with Hugo and publishes it to GitHub Pages using GitHub's official Pages Actions (`actions/configure-pages`, `actions/upload-pages-artifact`, `actions/deploy-pages`).

**One-time GitHub setup** (see repo Settings → Pages): set the Pages source to **GitHub Actions**.

Before your first deploy, update:

- `baseURL` in `config/_default/hugo.toml` (must match `https://<your-github-username>.github.io/`)
- Author name/bio/social links in `config/_default/languages.en.toml`

## Adding photos

- **Profile/headshot**: `assets/img/author.jpg`, referenced by `params.author.image` in `config/_default/languages.en.toml`. Hugo automatically resizes/crops this one.
- **Other photos** (project screenshots, blog images, etc.): put them in `static/images/` and reference them as `/images/your-file.jpg` in Markdown. Don't put photos in `data/` — that folder is reserved for structured data files (YAML/JSON/TOML) and Hugo will fail to build if it finds an image there.

## Color scheme

The site uses a custom **olive** color scheme defined in `assets/css/schemes/olive.css` (primary = olive green, secondary = warm gold, neutrals = warm stone). Selected via `colorScheme = "olive"` in `config/_default/params.toml`. Edit the RGB triples in that file to adjust the palette, or set `colorScheme` to any of the built-in Blowfish schemes (`avocado`, `forest`, `ocean`, `slate`, etc. — see `themes/blowfish/assets/css/schemes/`).

