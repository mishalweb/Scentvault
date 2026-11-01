# ScentVault · Phase 1

**A modern, single-store perfume and decant e-commerce project** built with Next.js, NestJS, PostgreSQL, and **Prisma ORM 7**.

> **Phase 1 foundation:** public website, responsive catalog, search/filters, fragrance detail with interactive volume selector, PostgreSQL models, seed dataset, API docs, and starter tests. **This is not a transactional shop yet.** Authentication, cart, checkout, real order processing, stock reservations and admin dashboards are scheduled for later phases.

## Prerequisites

- **Node.js 22.18+**, npm 10+, internet connection for the first `npm install`.
- Docker Desktop (Windows/macOS) or Docker Engine + Compose (Linux), **or** an existing PostgreSQL 16+ server.
- Git is useful but optional.

## Quick start (from repository root)

```bash
# 1. Extract ZIP and open the scentvault-phase1 folder
npm install

# 2. Copy safe local development environment templates
npm run setup:env

# 3. Start PostgreSQL (requires Docker)
npm run db:up

# 4. Generate typed Prisma 7 client
npm run db:generate

# 5. Apply committed initial SQL migration
npm run db:migrate

# 6. Load six fictional fragrances and variants
npm run db:seed

# 7. Start both apps in watch mode
npm run dev
```

**Open:**

- Website: http://localhost:3000
- Perfume collection: http://localhost:3000/shop
- API: http://localhost:4000/api/v1/health
- Database health: http://localhost:4000/api/v1/health/db
- Swagger documentation: http://localhost:4000/api/docs

To verify after both apps start, run `npm run smoke` from a **second terminal**.

**Windows PowerShell:** the commands above run the same way once Node.js and Docker Desktop are installed. Run them in the extracted project's root directory. `npm run setup:env` copies both files on Windows automatically.

## Start order matters

1. Database must accept connections **before** you run `db:migrate`, `db:seed`, or the API.
2. `db:generate` must run **before** NestJS can compile, because its Prisma client is generated into `apps/api/src/generated/prisma` (ignored by Git).
3. `npm run dev` starts backend and frontend with `concurrently`.

If PostgreSQL is already running on port **5432**, stop the other instance or update both `docker-compose.yml` port mapping and `DATABASE_URL` in `apps/api/.env`.

## Directory structure

```text
scentvault-phase1/
├── apps/
│   ├── api/
│   │   ├── prisma/
│   │   │   ├── migrations/20261101000000_init/migration.sql
│   │   │   ├── schema.prisma
│   │   │   └── seed.ts
│   │   ├── src/
│   │   │   ├── catalog/       # GET brand and fragrance APIs
│   │   │   ├── health/        # readiness and DB checks
│   │   │   ├── prisma/        # adapter + Nest provider
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── .env.example
│   │   └── prisma.config.ts
│   └── web/
│       ├── public/images/    # Original SVG fictional perfume illustrations
│       └── src/
│           ├── app/          # App Router pages
│           ├── components/   # Header, footer, cards and selector
│           └── lib/          # Typed API access and BDT formatting
├── docs/                    # Blueprint, ERD, architecture, testing
├── scripts/                 # env setup and HTTP smoke checks
├── docker-compose.yml
└── package.json             # npm workspaces and scripts
```

## Runtime variables

`apps/api/.env` (copied from `.env.example`):

```env
DATABASE_URL="postgresql://scentvault:scentvault_dev_password@localhost:5432/scentvault?schema=public"
PORT=4000
FRONTEND_URL=http://localhost:3000
NODE_ENV=development
```

`apps/web/.env` (copied from `.env.example`):

```env
API_INTERNAL_URL=http://localhost:4000/api/v1
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

**Security:** credentials above are for local development ONLY. Never commit `.env` files or expose these credentials on a live service.

## Database commands

| Command | Purpose |
| --- | --- |
| `npm run db:up` | Start Docker PostgreSQL |
| `npm run db:down` | Stop Docker containers; preserve named volume |
| `npm run db:generate` | Generate Prisma 7 client |
| `npm run db:migrate` | Apply committed migrations with `prisma migrate deploy` |
| `npm run db:status` | Show applied migration status |
| `npm run db:dev` | Create/apply new migrations during development |
| `npm run db:seed` | Idempotently seed demo catalog |

**Warning:** `docker compose down -v` deletes the persisted development database. Don't use it if you need existing data.

## Test and build

```bash
npm run typecheck
npm test
npm run build
npm run smoke  # after `npm run dev` is running
```

## Example API requests

```bash
curl http://localhost:4000/api/v1/fragrances
curl 'http://localhost:4000/api/v1/fragrances?brand=noir-atelier&q=velvet'
curl http://localhost:4000/api/v1/fragrances/velvet-ember
curl http://localhost:4000/api/v1/brands
```

## Scope and honest limitations

- **Implemented in source:** Next.js responsive UI, read-only Nest API, Prisma schema, baseline SQL migration, SVG assets, reusable typed API client, seed data, basic Jest unit tests, smoke script, Docker DB configuration and documentation.
- **Not implemented:** registration/login; cart/wishlist persistence; coupon checks; checkout; paying; inventory reservations/stock transactions; real product images; analytics, role-based admin; live shipping. Buttons for unfinished commerce features are intentionally disabled.
- **Runtime verification:** this ZIP was assembled where npm registry access and Docker were unavailable. Static source validation was run, but successful dependency installation, Prisma migration execution and end-to-end UI operation **have not been demonstrated**. Please execute the setup and verification steps above in your environment.

## Future phases

1. Foundation (this delivery)
2. Authentication and roles
3. Staff catalog CRUD
4. Decant inventory and audit
5. Cart and transactional checkout
6. Fulfillment and reviews
7. Admin analytics
8. Deployment and full test suite

See `docs/PROJECT_BLUEPRINT.md`, `docs/ARCHITECTURE.md`, `docs/erd.mmd`, and `docs/TESTING.md`.
