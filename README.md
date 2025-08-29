# GitHub Repo Tree Browser (GitHub Pages)

A single-file, client-only explorer for browsing a public GitHub repository as a collapsible tree on **GitHub Pages**.
It previews images inline, shows small text files, and links to **Raw** / **View on GitHub**.

> **No server or token required.** Uses the public GitHub Contents API with unauthenticated requests.

## Features

* 🗂️ Collapsible directory tree (lazy-loads subfolders)
* 🖼️ Inline image preview (PNG, JPG, GIF, WEBP, AVIF, SVG)
* 📄 Text preview for small files (≤ \~1 MB; md/txt/json/yml/csv/js/ts/css/html/xml/sh/py)
* 🧭 Breadcrumb navigation
* 🔗 Deep linking to a specific file or folder (`?path=...`)

  * **Safe fallback** when embedded/sandboxed: uses `#path=...` if the environment disallows `history.replaceState`
* 🔍 “View on GitHub” and “Raw” quick links
* ⚙️ Switch owner / repo / branch from the header controls

## Live usage

If you place `index.html` at the repo root and enable Pages on the `main` branch:

```bash
https://<your-username>.github.io/<your-repo>/
```

For your current setup (owner/repo defaults are already set in the file):

```bash
https://michalferber.github.io/resources/
```

## Quick start

1. **Add the file**

   * Create `index.html` in your repo (or `docs/index.html` if you prefer the `/docs` Pages source).
   * Paste the provided HTML file.

2. **Enable GitHub Pages**

   * Repo ➜ **Settings** ➜ **Pages**
   * **Source**: `main` / `/ (root)` (or `main` / `/docs` if you used `/docs`)

3. **Open**

   * Visit your Pages URL and click folders/files to browse and preview.

## Configuration

The header has editable fields:

* **Owner** (default: `MichalAFerber`)
* **Repo** (default: `resources`)
* **Branch** (default: `main`)
* **Load** (reloads the tree with the new settings)

These can also be set via URL parameters:

| Param    | Example                           | Notes                              |
| -------- | --------------------------------- | ---------------------------------- |
| owner    | `?owner=MichalAFerber`            | GitHub user/org                    |
| repo     | `&repo=resources`                 | Repository name                    |
| branch   | `&branch=main`                    | Branch to browse                   |
| path     | `&path=folder/subfolder/file.png` | Deep link to a file or folder      |
| selftest | `&selftest=1`                     | Runs console self-tests (optional) |

### Examples

* Open a folder:
  `...?path=images/social`
* Open a file:
  `...?path=social/facebook-mesenger.png`

> If the page is embedded (e.g., `about:srcdoc` iframe), `path` is stored in the **hash** (`#path=...`) instead of the query string so the URL handling stays safe.

## What’s previewed

* **Images:** PNG, JPG, JPEG, GIF, WEBP, AVIF, SVG (rendered inline)
* **Text:** MD, TXT, JSON, YML/YAML, CSV, JS/TS, CSS, HTML, XML, SH, PY (≤ \~1 MB)
* **Other/big files:** direct **Raw** and **View on GitHub** links

## Limits & notes

* **Public repos only** (no token used). Private repos would require a server or careful token handling (not recommended client-side).
* **Rate limit:** \~60 requests/hour per IP (unauthenticated). The UI is efficient, but very heavy browsing may hit the limit.
* **Large text files:** Not previewed; use **Raw** link.
* **Symlinks / submodules:** Not resolved (mirrors GitHub API behavior).

## Deep-linking & embedding

* On GitHub Pages (normal origin), the app updates the **query string** (`?path=...`) via `history.replaceState` for clean deep links.
* In sandboxed/embedded contexts (e.g., `about:srcdoc` iframes), browsers block `replaceState`. The app **falls back** to `#path=...` and listens to `hashchange` so links still work.

## Development / self-tests

You can run a small browser-console self-test of the URL state helpers:

```bash
https://<your-pages-url>/?selftest=1
```

Open the dev console to see:

* `✔ set/get path roundtrip` if the helpers function correctly in your environment.

## Troubleshooting

* **SecurityError: replaceState on about\:srcdoc**
  You’re previewing in a sandboxed environment. This is expected; the app automatically falls back to `#path=...`. On GitHub Pages, query string deep links will work normally.
* **Rate limit exceeded**
  Wait a few minutes and try again, or narrow your browsing scope.
* **Images not loading**
  Check that the file path is correct and the file is stored in the same repo/branch you selected.

## Customization ideas

* Add a lightbox for image previews
* Remember expanded folders in `localStorage`
* Filter/search within the current folder
* Keyboard navigation (↑/↓/←/→, Enter)

## Security

* No OAuth/token usage; only public API endpoints (`/repos/:owner/:repo/contents/...`) and `raw.githubusercontent.com` are called.
* All requests are **read-only**.

## License

[MIT](LICENSE)
