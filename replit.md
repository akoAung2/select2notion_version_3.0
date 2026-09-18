# Select2Notion AI — Trading Journal

## Overview
AI-powered trading journal SaaS that syncs trades to Notion, surfaces Gemini-driven analytics, polls financial news feeds, and sends Telegram notifications. Users subscribe via Tally; auth is handled through Notion OAuth backed by Firebase.

## Stack
- **Frontend**: React + Vite + Tailwind CSS + Radix UI (`artifacts/trading-journal/`)
- **Backend**: Express + TypeScript, built with esbuild (`artifacts/api-server/`)
- **Shared libs**: Zod schemas, OpenAPI spec, generated React Query client (`lib/`)
- **Persistence**: Firebase (auth/user data), Upstash Redis (sessions/caching), Notion (trade storage)
- **Package manager**: pnpm (workspace monorepo)

## How to Run

### Install dependencies (once)
```bash
pnpm install
```

### Workflows (managed by Replit)
| Workflow | Command | Port | Output |
|---|---|---|---|
| `artifacts/trading-journal: web` | `cd artifacts/trading-journal && pnpm run dev` | 5000 | webview |
| `artifacts/api-server: API Server` | `cd artifacts/api-server && pnpm run dev` | 8080 | console |

The frontend proxies `/api` requests to `http://localhost:8080` in dev.

## Required Secrets
All configured in Replit Secrets:

| Secret | Purpose |
|---|---|
| `SESSION_SECRET` | Express session signing |
| `FIREBASE_PROJECT_ID` / `FIREBASE_CLIENT_EMAIL` / `FIREBASE_PRIVATE_KEY` / `FIREBASE_PRIVATE_KEY_ID` | Firebase Admin SDK |
| `NOTION_CLIENT_ID` / `NOTION_CLIENT_SECRET` | Notion OAuth |
| `UPSTASH_REDIS_REST_URL` / `UPSTASH_REDIS_REST_TOKEN` | Session store & caching |
| `TELEGRAM_BOT_TOKEN` / `TELEGRAM_ADMIN_CHAT_ID` | Telegram notifications |
| `GOOGLE_API_KEY` | Gemini AI analysis |
| `TALLY_FORM_URL` / `TALLY_WEBHOOK_SECRET` | Subscription management |
| `ALPHA_VANTAGE_API_KEY` / `FMP_API_KEY` / `FRED_API_KEY` / `NEWS_API_KEY` / `BEA_API_KEY` / `BLS_API_KEY` | Financial data feeds |

## Notes
- Telegram bot is intentionally disabled in dev mode (dev domain can't receive webhooks)
- The API server serves the built frontend static files in production (`artifacts/trading-journal/dist/public`)
- Redis ping is checked on startup; a failed ping indicates missing/wrong Upstash credentials
- Source zips (`artifacts.zip`, `lib.zip`) in the root are the import originals — safe to delete

## User Preferences
