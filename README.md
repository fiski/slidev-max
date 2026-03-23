# Slidev Presentations

A collection of Slidev presentations hosted on GitHub Pages.

## Presentations

| Name | Folder | Dev port |
|------|--------|----------|
| Q3 Product Roadmap Review | `presentations/sample-presentation` | 3004 |

## Development

Install dependencies once at the root:

```bash
npm install
```

Run a presentation locally:

```bash
npm run dev:sample
```

Build for production:

```bash
npm run build:sample
```

Preview the production build:

```bash
npm run preview:sample
```

## Adding a new presentation

1. Create `presentations/<your-slug>/slides.md`
2. Create `presentations/<your-slug>/public/assets/` for images
3. Add `dev`, `build`, and `preview` scripts to `package.json`
4. Add a card to `index.html`
5. Add a build step to `.github/workflows/deploy-pages.yml`
