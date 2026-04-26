# Resources · Repo Browser (GitHub Pages)

A single-file, client-only explorer for browsing a public GitHub repository as a collapsible tree on **GitHub Pages**.
It previews images inline, shows small text files, and gives you one-click **raw**, **CDN**, and **GitHub** links.

> **No server required.** Uses the public GitHub Contents API; works unauthenticated, or with an optional Personal Access Token kept entirely in your browser.

## Features

* 🗂️ Collapsible directory tree (lazy-loads subfolders, remembers expanded state)
* 🔎 Live filter for the current folder (Esc clears)
* 🖼️ Inline image preview (PNG, JPG, GIF, WEBP, AVIF, SVG) + lightbox with keyboard nav
* 📄 Text preview for small files (≤ \~1 MB; md/txt/json/yml/csv/js/ts/css/html/xml/sh/py)
* 🧭 Breadcrumb navigation
* 🔗 Deep linking to a specific file or folder (`?path=...`)
  * **Safe fallback** when embedded/sandboxed: uses `#path=...` if the environment disallows `history.replaceState`
* 📋 One-click copy of **raw URL**, **jsDelivr CDN URL**, or raw bytes (image/text)
* 🌗 Light / Dark / System theme toggle (persisted)
* 🔑 Optional GitHub PAT (browser-only) to lift the rate limit from 60 → 5000 req/hr
* ⚙️ Switch owner / repo / branch from the header controls
* 📣 Open Graph + Twitter card meta tags for nicer link previews

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
* **Token** (optional GitHub PAT; see [Authentication](#authentication))
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

## Copying URLs

Three buttons on the toolbar:

| Button     | What it copies                                                                 | When to use                                            |
| ---------- | ------------------------------------------------------------------------------ | ------------------------------------------------------ |
| Copy URL   | `https://raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}`             | Direct file URL, served by GitHub                      |
| Copy CDN   | `https://cdn.jsdelivr.net/gh/{owner}/{repo}@{branch}/{path}`                   | Faster, cached embedding (blogs, README images, sites) |
| Copy Raw   | The actual bytes (image → clipboard image, text → clipboard text)              | Pasting into chat / docs                               |

If your browser blocks binary clipboard writes (sandboxed contexts, non-HTTPS), **Copy Raw** falls back to copying the raw URL.

## Filtering

Type into the **Filter this folder…** box at the top of the tree to narrow visible items in the current folder by name. Press **Esc** to clear. The filter resets automatically when you navigate to another folder.

## Authentication

By default the app calls the GitHub API anonymously, which is capped at **\~60 requests/hour per IP**.

Optionally, paste a GitHub **Personal Access Token** (fine-grained or classic; only `public_repo` read scope is needed) into the **Token** field in the header to raise the cap to **5000 req/hr**. The token:

* Is stored only in your browser's `localStorage` under the key `repoTree:pat`
* Is sent only as an `Authorization: Bearer …` header to `api.github.com`
* Is never written into URLs, query strings, history, or anywhere shareable

> **Security note:** anyone with access to your browser profile can read this token. Use a low-scope, easily-revocable PAT, and clear the field when you're done if you share the device.

A 401 response (invalid/expired token) shows a dismiss-able banner so you know to clear or replace it.

## Limits & notes

* **Public repos only.** Private repos technically work if your PAT has access, but the app is designed for public browsing.
* **Rate limit:** \~60 requests/hour per IP unauthenticated; 5000/hr with a PAT.
* **Large text files:** Not previewed; use **Raw** link.
* **Symlinks / submodules:** Not resolved (mirrors GitHub API behavior).
* **jsDelivr cache:** CDN URLs may be cached up to ~12 hours. Use the raw URL if you need an instant update.

## Deep-linking & embedding

* On GitHub Pages (normal origin), the app updates the **query string** (`?path=...`) via `history.replaceState` for clean deep links.
* In sandboxed/embedded contexts (e.g., `about:srcdoc` iframes), browsers block `replaceState`. The app **falls back** to `#path=...` and listens to `hashchange` so links still work.

## Development / self-tests

You can run a small browser-console self-test of the URL state helpers:

```bash
https://<your-pages-url>/?selftest=1
```

Open the dev console to see roundtrip checks for URL state, open-folder persistence, clipboard capability detection, and that `rawURL` / `ghURL` / `cdnURL` correctly percent-encode tricky path segments (e.g. spaces, `&`).

## Troubleshooting

* **SecurityError: replaceState on about\:srcdoc**
  You’re previewing in a sandboxed environment. This is expected; the app automatically falls back to `#path=...`. On GitHub Pages, query string deep links will work normally.
* **Rate limit exceeded**
  Wait a few minutes and try again, or narrow your browsing scope.
* **Images not loading**
  Check that the file path is correct and the file is stored in the same repo/branch you selected.

## Customization ideas

* Recursive (deep) filter that searches across all loaded subfolders
* Keyboard navigation (↑/↓/←/→, Enter) for the tree
* Bulk-copy raw/CDN URLs from a multi-select on the thumbnail grid
* Recently-copied history pinned to the footer

## Security

* No OAuth flow; only `api.github.com`, `raw.githubusercontent.com`, and `cdn.jsdelivr.net` are called.
* All requests are **read-only**.
* If a PAT is provided it is stored only in `localStorage` and sent only as an `Authorization` header to `api.github.com` — never to jsDelivr or anywhere else. See the [Authentication](#authentication) section for the trade-off.

## License

[MIT](LICENSE)
