# Rodrigo Aroso Portfolio

Portfolio website built with [Astro](https://astro.build/).

## Requirements

- Node.js 22
- npm
- Docker Engine with Docker Compose for staging deployment

## Local development

Install the dependencies and start the development server:

```bash
npm ci
npm run dev
```

Create and preview a production build before opening a pull request:

```bash
npm run build
npm run preview
```

## Project structure

- `src/pages/` — website routes and page content
- `src/components/` — reusable Astro components
- `src/styles/` — global styling
- `public/` — images, videos, and fonts

## Branch and deployment workflow

- `master` is the production branch.
- `staging` is the integration branch and is deployed to [ra.wastelabs.net](https://ra.wastelabs.net).
- Short-lived `feature/*`, `chore/*`, and `fix/*` branches are created from `staging` and merged back into `staging` when ready.
- After changes have been validated in staging, `staging` can be promoted to `master` for production.

The staging site runs on the Raspberry Pi homelab with Docker. Cloudflare Tunnel exposes the container at `ra.wastelabs.net` without opening it directly to the internet.

Typical change flow:

```text
feature/*, chore/*, or fix/*
              ↓
           staging
              ↓
     ra.wastelabs.net
              ↓
            master
```

## Manual staging deployment

After merging changes into `staging`, update the checkout on the Raspberry Pi and rebuild the container:

```bash
cd /opt/homelab/apps/rodrigo-portefolio
git fetch origin
git switch staging
git pull --ff-only origin staging
docker compose up -d --build
docker compose ps
docker compose logs --tail=100 portfolio
```

Cloudflare Tunnel forwards `ra.wastelabs.net` to the local service exposed by `compose.yaml` on port `8081`.
