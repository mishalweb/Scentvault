# ScentVault — Design Pack

ScentVault is a **planned** full-stack single-store perfume and decant e-commerce application.

This ZIP is an **architecture/design pack only**, not a runnable web application. It contains:

- `PROJECT_BLUEPRINT.md`: product requirements, features, roles, frontend routes, REST API, inventory rules, security and build roadmap.
- `apps/api/prisma/schema.prisma`: initial 22-model PostgreSQL/Prisma ORM 7 schema. **Not yet migration-tested.**
- `docs/erd.mmd`: Mermaid entity-relationship diagram.

## Next step

Create Next.js (`apps/web`), NestJS (`apps/api`), PostgreSQL Docker Compose, Prisma 7 configuration and migrations, then implement the modules in the order described in `PROJECT_BLUEPRINT.md`.

**Important:** Do not apply this schema to an existing database or production system without reviewing and testing the migrations. Purchase/payment and stock mutation require transactional, idempotent implementation.
