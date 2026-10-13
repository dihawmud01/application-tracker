# Career Tracker — SDD baseline v1.0

Stack: Next.js, NestJS REST API, PostgreSQL + Prisma, external AI API through backend, Docker Compose local development. Architecture: monorepo with a modular-monolith backend. Auth: one local account with secure session cookie. Documents: persistent local volume outside public web root; metadata in PostgreSQL.

Read in order:
1. docs/01-srs.md
2. docs/02-architecture-and-data-model.md
3. prisma/schema.prisma
4. docs/03-api-contract.md
5. docs/04-sdd-plan.md
6. AGENTS.md

The schema is a baseline and must be validated with the exact pinned Prisma version before migration.
