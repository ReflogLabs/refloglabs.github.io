# Reflog Labs Website

Official source repository for [reflog-labs.github.io](https://reflog-labs.github.io).

## Overview

Reflog Labs is an interactive learning platform that teaches developers how open source contribution and Git workflows function in production environments.

The site is built with vanilla HTML, CSS, and modern client-side JavaScript. It dynamically polls public GitHub repositories in the `reflog-labs` organization to unlock available lab modules and display real-time commit activity.

## Structure

- `index.html`: Main single-page interactive curriculum site.
- `404.html`: Custom error page matching design system tokens.
- `og.png`: Open Graph preview image for social cards (1200x630).
- `assets/`: Static image and brand assets (`logo.svg`).
- `.github/workflows/deploy.yml`: GitHub Actions pipeline publishing the site to GitHub Pages.

## Local Development

You can preview the site locally using any static web server:

```bash
# Python built-in HTTP server
python -m http.server 8000

# Node http-server
npx http-server . -p 8000
```

Open `http://localhost:8000` in your browser.

## Testing and Mock Parameters

To run contract checks and verify DOM behaviors without making unauthenticated GitHub API calls, use Node.js and JSDOM:

```bash
# Inspect DOM states or run headless assertions with jsdom
npm install --no-save jsdom
```

The site supports development query parameters:
- `?mock=none`: Simulates zero public repositories (all lab cards locked).
- `?mock=01,02,04`: Unlocks cards 01, 02, and 04.
- `?mock=all`: Unlocks all lab cards.
- `?mock=offline`: Simulates an API network failure to verify error handling.

## Deployment

Pushes to the `main` branch trigger the GitHub Actions workflow in `.github/workflows/deploy.yml`, which bundles the production site into a `_site` directory and deploys it directly to GitHub Pages.

## License

MIT License. See [LICENSE](LICENSE) for details.
