## KrushiMitra AI Agent — Docker Setup

This guide shows how to run the app using Docker (production build). For local non-Docker development, use npm scripts as usual.

---

## Prerequisites

- Docker Desktop (or Docker Engine + Docker Compose)

---

## 1) Environment Variables

Create a `.env.local` file in the project root with your keys:

```bash
# .env.local

# Required
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_api_key_here
SARVAM_API_KEY=your_sarvam_ai_subscription_key_here
MANDI_PRICE_API_KEY=your_data_gov_in_api_key_here

# Optional (public)
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your_google_maps_api_key_here

# App config
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=Farmeasy
NODE_ENV=production
```

---

## 2) Run with Docker Compose (recommended)

```bash
# Build and start
docker compose up -d --build

# Tail logs
docker compose logs -f

# Stop and remove containers
docker compose down
```

- App will be available at: http://localhost:3000
- To change the host port, edit `ports:` in `docker-compose.yml` (e.g., `8080:3000`).

---

## 3) Run with Docker CLI (alternative)

```bash
# Build image
docker build -t krushimitra-ai-agent .

# Run container (loads secrets from .env.local)
docker run --env-file .env.local -p 3000:3000 --name krushimitra-ai-agent krushimitra-ai-agent
```

Open the app at: http://localhost:3000

---

## Notes

- Public `NEXT_PUBLIC_*` values are baked into the client bundle at build time. If you change them, rebuild the image.
- Private keys (non-`NEXT_PUBLIC`) are read at runtime; restart the container if they change.

---

## Troubleshooting

- Ensure `.env.local` exists in the project root before starting containers.
- If the port is busy, change the left-hand side of the port mapping:
  - Compose: update `ports: ["8080:3000"]`
  - CLI: use `-p 8080:3000`
- If dependencies seem missing after updates, run a clean rebuild:
  - `docker compose build --no-cache && docker compose up -d`
