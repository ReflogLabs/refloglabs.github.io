<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/og-dark.png#gh-dark-mode-only">
  <source media="(prefers-color-scheme: light)" srcset="assets/og-light.png#gh-light-mode-only">
  <img src="assets/og-light.png#gh-light-mode-only" alt="Reflog Labs Official Website Repository" width="100%">
</picture>

</div>

<br>

The production front-end repository powering [refloglabs.github.io](https://refloglabs.github.io), the interactive curriculum hub for Reflog Labs.

---

## Visual Preview

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/preview-hero-dark.png#gh-dark-mode-only">
  <source media="(prefers-color-scheme: light)" srcset="assets/preview-hero-light.png#gh-light-mode-only">
  <img src="assets/preview-hero-light.png#gh-light-mode-only" alt="Reflog Labs Platform Interface" width="100%">
</picture>

</div>

<br>

---

## Architecture and Design Philosophy

Reflog Labs teaches Git mechanics and production engineering workflows through structured terminal walkthroughs and downloadable guides. The website is engineered as a zero-dependency, single-page application with high-performance client-side rendering.

```text
[ Browser Client ]
        |
        +---> [ Static Host / GitHub Pages ]
        |         |-- index.html (Vanilla ES6, inline responsive styles, 60fps canvas)
        |         |-- 404.html (Branded not-found error shell)
        |         \-- og.png, assets/logo.svg
        |
        +---> [ GitHub REST API ]
                  \-- GET api.github.com/orgs/refloglabs/repos
                        \-- Unlocks curriculum cards dynamically as public labs publish
```

### Key Engineering Decisions

1. **Zero External Runtime Dependencies**: No frameworks, no build bundlers, and no compile step required for browser delivery.
2. **Dynamic Curriculum Hydration**: The site queries the public GitHub API on load to discover published repositories matching the `lab-NN-slug` naming convention. As new labs are published in the `refloglabs` organization, corresponding curriculum cards automatically unlock.
3. **Adaptive Dual-Theme Engine**: Full support for light and dark modes with persistent cookie storage and an animated circular ripple theme transition.
4. **SVG Favicon Dynamic Switching**: The browser tab favicon updates automatically with the active theme (`#FFFFFF` in light mode for dark tab bars, `#A3E635` in dark mode).

---

## Interactive Curriculum Platform

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/preview-curriculum-dark.png#gh-dark-mode-only">
  <source media="(prefers-color-scheme: light)" srcset="assets/preview-curriculum-light.png#gh-light-mode-only">
  <img src="assets/preview-curriculum-light.png#gh-light-mode-only" alt="Dynamic Curriculum Cards" width="100%">
</picture>

</div>

<br>

---

## Design System Tokens

| Semantic Token | Light Mode | Dark Mode | Usage |
|---|---|---|---|
| `Ground` | `#F5F7FA` | `#0B1016` | Viewport background |
| `Surface` | `#FFFFFF` | `#111922` | Cards, terminal windows, modal dialogs |
| `Border` | `#0E1030` | `#26323F` | High-contrast structural boundaries (1.2 pt) |
| `Ink` | `#0E1030` | `#E6EDF3` | Primary body typography and headers |
| `Muted Ink` | `#4A4F6B` | `#93A1B3` | Secondary labels, timestamps, metadata |
| `Accent` | `#2A2FD6` | `#A3E635` | Interactive controls, highlights, focus states |
| `Attention` | `#E8622C` | `#F07A45` | Action callouts and download triggers |

---

## Directory Structure

```text
refloglabs.github.io/
|-- .github/
|   \-- workflows/
|       \-- deploy.yml                  # Production GitHub Pages deployment pipeline
|-- assets/
|   |-- logo.svg                        # Canonical vector brand mark
|   |-- og-dark.png                     # Dark theme social banner
|   |-- og-light.png                    # Light theme social banner
|   |-- preview-hero-dark.png           # Dark mode hero screenshot
|   |-- preview-hero-light.png          # Light mode hero screenshot
|   |-- preview-curriculum-dark.png     # Dark mode curriculum grid screenshot
|   \-- preview-curriculum-light.png    # Light mode curriculum grid screenshot
|-- .gitignore                          # Local artifacts and editor exclusion rules
|-- .nojekyll                           # Bypasses Jekyll processing on GitHub Pages
|-- index.html                          # Main application entry point
|-- 404.html                            # Branded 404 page
|-- og.png                              # Open Graph fallback banner (1200x630)
|-- LICENSE                             # Open source license
\-- README.md                           # Repository documentation
```

---

## Local Development and Preview

Because the site uses vanilla standards, any local HTTP server can serve the project without compilation:

```bash
# Python 3
python -m http.server 8000

# Node.js
npx http-server . -p 8000
```

Open `http://localhost:8000` in any modern web browser.

---

## Testing and Development Mock Modes

The client-side API engine supports deterministic test query parameters, enabling offline development and contract testing without consuming unauthenticated GitHub API rate limits:

| Query Parameter | Target State | Expected Behavior |
|---|---|---|
| `?mock=none` | Zero published repos | All curriculum module cards render locked state. |
| `?mock=01,02,04` | Selective unlock | Cards 01, 02, and 04 unlock dynamically; others remain locked. |
| `?mock=all` | Complete catalog | Unlocks all curriculum cards across all tracks. |
| `?mock=offline` | Network partition | Simulates HTTP failure; displays non-blocking warning notification. |

---

## Deployment Pipeline

Deployment to production is completely automated via GitHub Actions:
- Pipeline configuration: `.github/workflows/deploy.yml`
- Trigger: Every push to the `main` branch.
- Destination: `https://refloglabs.github.io`
- Artifact payload: Bundles root static files into an optimized Pages artifact.

---

## License

MIT License. See [LICENSE](LICENSE) for details.
