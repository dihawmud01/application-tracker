# API Contract v1 draft

Base prefix `/api/v1`. JSON except upload/download. All endpoints except auth/bootstrap require a valid session.

## Auth
- `POST /auth/login`
- `POST /auth/logout`
- `GET /auth/session`
- One-time local account setup via documented CLI/bootstrap command; no public registration.

## Profile
- `GET|PATCH /profile`
- `GET|POST /profile/education`; `PATCH|DELETE /profile/education/:id`
- `GET|POST /profile/experiences`; `PATCH|DELETE /profile/experiences/:id`
- `GET|POST /profile/certifications`; `PATCH|DELETE /profile/certifications/:id`

## Skills/projects/documents
- `GET|POST /skills`
- `GET|POST /profile/skills`; `PATCH|DELETE /profile/skills/:id`
- `GET|POST /projects`; `GET|PATCH|DELETE /projects/:id`
- `POST|DELETE /projects/:id/skills/:skillId`
- `GET|POST /documents`; `GET /documents/:id/download`; `DELETE /documents/:id`

## Jobs/import
- `GET|POST /jobs`; `GET|PATCH|DELETE /jobs/:id`
- `GET|POST /jobs/:id/requirements`
- `PATCH|DELETE /jobs/:id/requirements/:requirementId`
- `POST /jobs/import-url`; `POST /jobs/import-text`
- `GET /imports/:id`; `POST /imports/:id/confirm`

## Applications/tasks
- `GET|POST /applications`; `GET|PATCH /applications/:id`
- `POST /applications/:id/status`
- `GET|POST /applications/:id/events`
- `GET|POST /applications/:id/tasks`
- `PATCH|DELETE /tasks/:id`

## AI/dashboard
- `POST /jobs/:id/analyze`
- `GET /analyses/:id`
- `GET /jobs/:id/analyses`
- `GET /dashboard/summary`
- `GET /dashboard/activity`

## Conventions
Use DTO validation, pagination, consistent errors, safe date/numeric validation, and server-side authorization. Status change + event creation is one transaction. Document access checks ownership. URL import returns a reviewable draft; no silent final save.
