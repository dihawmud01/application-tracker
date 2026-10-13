# SDD implementation plan

## Per-feature workflow
1. Read AGENTS.md, SRS, architecture, API contract, and relevant feature spec.
2. Identify ambiguities before coding.
3. Present affected files, schema/API changes, test plan, and acceptance criteria.
4. Implement one small vertical slice.
5. Run Prisma validation/migration, tests, lint, and typecheck; report only checks actually run.
6. Review diff for security, data integrity, scope creep, and spec drift.
7. Update docs after approved changes; commit.

## Definition of Ready
A feature has an ID, user story, scope, business rules, API/data changes, error cases, dependencies, and acceptance criteria.

## Definition of Done
Acceptance criteria pass; relevant tests and static checks are run; docs match implementation; no secrets are committed; known limitations are reported.

## Phases
0. Bootstrap Git monorepo, Next.js, NestJS, Docker Compose PostgreSQL, env validation, health endpoint, lint/test scripts.
1. Validate Prisma schema, migrate, PrismaModule, account bootstrap, login/logout/session cookie, auth tests.
2. Profile, education, experience, certifications, skills, projects CRUD.
3. Document upload/download/delete, persistent volume, validation and authorization.
4. Manual jobs and requirements CRUD; then URL/pasted-text import drafts with SSRF controls.
5. Applications, transactional status history, tasks, dashboard.
6. AI provider adapter and structured extraction draft/review.
7. Job-fit analysis, versioned scoring, evidence, coverage, immutable snapshots.
8. Export, backup/restore drill, security review, accessibility, end-to-end test.

## First agent task
Bootstrap repository/runtime only. Do not implement product features or full database migration until bootstrap is reviewed.
