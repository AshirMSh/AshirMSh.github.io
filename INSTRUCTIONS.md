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

```powershell
# New blog post
hugo new content posts/my-new-post.md

# New project write-up
hugo new content projects/my-new-project.md
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
| Homepage intro | `content/_index.md` |
| About / career story | `content/about/_index.md` |
| Projects listing intro | `content/projects/_index.md` |
| Blog listing intro | `content/posts/_index.md` |
| An existing project or post | the matching `.md` file under `content/projects/` or `content/posts/` |

## 5. Add images

- Put photos in `static/images/` and reference them in Markdown as `/images/filename.jpg`:
  ```markdown
  ![Alt text](/images/filename.jpg)
  ```
- To replace your profile headshot, overwrite `assets/img/author.jpg` (this one gets auto-resized/optimized by Hugo).
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
- Other look-and-feel options are in `config/_default/params.toml`:
  - `colorScheme` — which scheme file to use (currently `"olive"`)
  - `defaultAppearance` — `"light"` or `"dark"`
  - `homepage.layout` — `"hero"` (current), or `"profile"`, `"card"`, `"background"`, `"page"`
  - `header.layout`, `article.*`, `list.*` — control headers, article pages, and list pages

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
