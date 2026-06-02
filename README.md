# resources

Static assets for personal projects: branding kits, logos, wallpapers, and social images. Public so they can be hot-linked or cloned as needed.

> Looking for the **GitHub Tree Browser** tool that used to live here? It moved to its own repo:
> **[MichalAFerber/github-tree-browser](https://github.com/MichalAFerber/github-tree-browser)** — live at
> [michalaferber.github.io/github-tree-browser](https://michalaferber.github.io/github-tree-browser/).

## Layout

```
resources/
├── company_branding_kits/   ← brand assets for third-party companies (Apple, Google, Proton, etc.)
├── logos/                   ← personal/project logos
├── socials/                 ← social media avatars + headers
├── wallpaper/               ← desktop wallpapers
├── favicon.ico              ← site favicon used by personal projects
└── index.html               ← redirect to the GitHub Tree Browser
```

## Hot-linking

Files can be hot-linked via raw.githubusercontent.com or jsDelivr:

```
https://raw.githubusercontent.com/MichalAFerber/resources/main/<path>
https://cdn.jsdelivr.net/gh/MichalAFerber/resources@main/<path>
```

The GitHub Tree Browser at the link above is the easiest way to browse the tree and grab raw/CDN URLs with one click.

## License

MIT — see [LICENSE](LICENSE).
