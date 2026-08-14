# Site Cheat Sheet

Quick reference for editing, running, and publishing this Hugo + Blowfish site.

## 1. One-time setup

```powershell
git clone --recurse-submodules <your-repo-url>
cd AshirWebsiteHugo
```

Already cloned but missing the theme? `git submodule update --init --recursive`

Hugo not on your PATH? Add it once (adjust the path to wherever it's installed — on this machine it's `C:\Users\SESA800407\AppData\Local\Microsoft\WinGet\Packages\Hugo.Hugo.Extended_Microsoft.Winget.Source_8wekyb3d8bbwe`):

```powershell
$hugoDir = "C:\Users\SESA800407\AppData\Local\Microsoft\WinGet\Packages\Hugo.Hugo.Extended_Microsoft.Winget.Source_8wekyb3d8bbwe"
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
[Environment]::SetEnvironmentVariable("Path", "$userPath;$hugoDir", "User")
```

**Important:** this only affects *new* terminals. Close every open terminal/PowerShell window (and restart VS Code if `hugo` still isn't found) so the next one you open picks up the change. To use `hugo` immediately in your *current* terminal without restarting anything:

```powershell
$env:Path += ";C:\Users\SESA800407\AppData\Local\Microsoft\WinGet\Packages\Hugo.Hugo.Extended_Microsoft.Winget.Source_8wekyb3d8bbwe"
```

(this temporary fix only lasts for that one terminal session)

## 2. Run the site locally

```powershell
hugo server --buildDrafts --buildFuture
```

Open **http://localhost:1313**. It live-reloads whenever you save a file. Stop with `Ctrl+C`.

## 3. Add content

The site has three sections, each with its own look:

```powershell
# New career milestone / project write-up (journey theme)
hugo new content vocation/my-new-project.md

# New philosophy essay or quote (Isfahan-pattern + serif reading theme)
hugo new content reflections/my-new-essay.md

# New travel or community story (Pinterest-style board)
hugo new content wanderings/my-new-story.md
```

Each file starts with front matter like:

```yaml
---
title: "My Project"
date: 2026-08-13
draft: false          # set false to make it public
tags: ["python", "optimization"]
summary: "One sentence describing it."
showTableOfContents: false
---
```

Write the body in Markdown below the `---`. Save — the local server updates instantly.

## 4. Edit existing pages

| Page | File |
|---|---|
| Homepage intro + quote | `content/_index.md` (the headline/bio quote itself lives in `config/_default/languages.en.toml`, see step 6) |
| Vocation intro, timeline, education, skills | `content/vocation/_index.md` |
| An existing career milestone / project | the matching `.md` file under `content/vocation/` |
| Reflections listing intro | `content/reflections/_index.md` |
| An existing essay/quote | the matching `.md` file under `content/reflections/` |
| Wanderings listing intro | `content/wanderings/_index.md` |
| An existing travel/community story | the matching `.md` file under `content/wanderings/` |

The career timeline on the Vocation page uses Hugo's built-in `timeline` shortcode:

```markdown
{{</* timeline */>}}
{{</* timelineItem icon="star" header="Role Title" badge="2024 – Present" subheader="Company · City" md="true" */>}}
- Bullet point achievements go here (Markdown works because of `md="true"`).
{{</* /timelineItem */>}}
{{</* /timeline */>}}
```

Icons come from `themes/blowfish/assets/icons/` (e.g. `code`, `star`, `graduation-cap`, `lightbulb`, `globe`, `heart`).

## 5. Add images

- Put photos in `static/images/` and reference them in Markdown as `/images/filename.jpg`:
  ```markdown
  ![Alt text](/images/filename.jpg)
  ```
- To replace your profile headshot, overwrite `assets/img/author.jpg` (this one gets auto-resized/optimized by Hugo).
- For a **Wanderings** entry, drop the photo next to its Markdown file as a *page bundle* so it becomes the card's cover image automatically:
  ```
  content/wanderings/my-trip/
    index.md          <- front matter + story (rename my-trip.md to my-trip/index.md)
    cover.jpg          <- any image with "feature"/"cover"/"thumbnail" in the name is picked up
  ```
  The more photos you add across entries, the more the Pinterest-style board (masonry columns) fills in.
- **Never** put images in a folder named `data/` — that's a reserved Hugo folder for structured data files (YAML/JSON/TOML) only, and it will break the build.

## 6. Change your name, bio, links

Edit `config/_default/languages.en.toml`:

```toml
[params.author]
  name = "..."
  headline = "..."
  bio = "..."
  email = "..."
  links = [
    { linkedin = "..." },
    { github = "..." },
    { email = "mailto:..." },
  ]
```

## 7. Change the style / color scheme

- Colors live in `assets/css/schemes/olive.css` — each line is an `R, G, B` triple; edit the numbers to shift the palette.
- Fonts, border removal, and each section's distinct look live in `assets/css/custom.css`:
  - Sitewide font is **Poppins**; Reflections essays switch to the serif **Lora** for a Substack-style reading experience.
  - Reflections gets a subtle geometric lattice pattern (an Isfahan-tilework nod) behind the header and page background.
  - Wanderings turns its card grid into Pinterest-style masonry columns with portrait-cropped thumbnails.
  - Card/thumbnail borders are intentionally suppressed here (`.article-link--card`, etc.) for an open, borderless look — tweak or remove those rules if you want borders back.
  - Each page's `<body>` gets a `site-section-<name>` class (added in `layouts/_default/baseof.html`) — use that to scope any new CSS to just one section.
- Other look-and-feel options are in `config/_default/params.toml`:
  - `colorScheme` — which scheme file to use (currently `"olive"`)
  - `defaultAppearance` — `"light"` or `"dark"`
  - `homepage.layout` — `"hero"` (current), or `"profile"`, `"card"`, `"background"`, `"page"`
  - `header.layout`, `article.*`, `list.*` — control headers, article pages, and list pages
  - Per-section look (cardView, groupByYear, showTableOfContents, etc.) can be overridden right in each section's `_index.md` front matter

## 8. Change the navigation menu

Edit `config/_default/menus.en.toml` — reorder items by changing `weight`, or add a new `[[main]]` block.

## 9. Publish / go live

Deployment is automatic via GitHub Actions ([.github/workflows/hugo.yml](.github/workflows/hugo.yml)): every push to `main` builds and deploys to GitHub Pages.

```powershell
git add -A
git commit -m "Describe your change"
git push
```

Check the **Actions** tab on GitHub for build status, then visit your `https://<username>.github.io/` URL a minute or two later.

One-time GitHub setup (only needed once per repo): **Settings → Pages → Source → GitHub Actions**.

## 10. Update the Blowfish theme (occasionally)

```powershell
cd themes/blowfish
git fetch --tags
git checkout <latest-tag>
cd ../..
git add themes/blowfish
git commit -m "Update Blowfish theme"
git push
```

## Common gotchas

- A page won't show up → check `draft: false` in its front matter.
- Future-dated content won't show in production → either change the date or run locally with `--buildFuture`.
- Build fails mentioning `data/...unmarshal` → you put a non-data file (image, etc.) inside `data/`. Move it to `static/` or `assets/` instead.
- Colors look off → confirm `colorScheme = "olive"` in `params.toml` matches the filename `assets/css/schemes/olive.css`.
- `hugo : The term 'hugo' is not recognized...` → PATH was updated but this terminal predates the change. Open a brand-new terminal window/tab (or restart VS Code), or use the one-liner in step 1 to fix the current session only.
