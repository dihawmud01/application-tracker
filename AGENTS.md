# Coding Agent Rules

- Read SRS, architecture/data model, API contract, security notes, and relevant feature spec before coding.
- Approved specifications are source of truth. Do not invent requirements or silently change business rules.
- Before coding, summarize scope, affected files, DB/API changes, tests, and acceptance criteria.
- Implement only the approved small scope.
- Keep DB access in NestJS. Use DTO validation and Prisma migrations.
- Use transactions for status update + event.
- Treat AI output as untrusted; validate it. Compute job-fit score in backend.
- Keep secrets backend-only; never commit `.env`; never store files in public web root.
- Protect URL import against SSRF and never bypass website access controls.
- Run relevant tests/lint/typecheck/schema validation and report actual outcomes.
- Update docs for approved changes. Do not introduce infrastructure/dependencies without rationale.
