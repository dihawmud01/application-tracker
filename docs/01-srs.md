# Career Tracker SRS v1.0

Status: baseline for implementation. Primary user: one account owner. UI language: Indonesian.

## Purpose
Manage a personal career profile, job opportunities, applications, recruitment events, tasks, and explainable AI-assisted job-fit analysis.

## MVP scope
- One local account; login/logout using secure session cookie.
- Profile, education, experience, certifications, skills, projects, and documents.
- Manually create/edit jobs and requirements.
- Optionally import a public job URL or pasted description as a draft; user reviews before final save.
- One application per saved job, with status history and tasks.
- Dashboard from persisted data.
- On-demand AI analysis with weighted criteria, evidence, gaps, unknowns, and recommendations.
- Export and documented backup/restore.
- AI is optional: manual flows work if provider is unavailable.

## Out of scope
Multi-user SaaS, public registration, auto-apply, bypassing CAPTCHA/login/paywall/anti-bot protections, mass scraping, microservices, Kubernetes, Redis/queues, cloud deployment, model fine-tuning.

## Functional requirements
| ID | Requirement | Priority |
|---|---|---|
| FR-001 | Login/logout for one local account; protected endpoints require a valid session. | Must |
| FR-002 | CRUD profile, education, experience, certifications. | Must |
| FR-003 | CRUD skills, proficiency, and evidence. | Must |
| FR-004 | CRUD projects and project-skill evidence. | Must |
| FR-005 | Upload/download/delete documents through authenticated endpoints. | Must |
| FR-006 | CRUD jobs and job requirements. | Must |
| FR-007 | Import URL/pasted text as a reviewable draft. | Should |
| FR-008 | Create at most one application per job. | Must |
| FR-009 | Update application status and preserve recruitment-event history. | Must |
| FR-010 | Track tasks and due dates. | Should |
| FR-011 | Dashboard shows actual persisted data. | Must |
| FR-012 | Run job-fit analysis only on explicit user action. | Must |
| FR-013 | Show score, coverage, criterion weights/scores, evidence, gaps, unknowns, recommendations. | Must |
| FR-014 | Store immutable analysis input snapshot and provider/model/prompt/scoring versions. | Must |
| FR-015 | Export data and test backup/restore. | Should/Must before routine use |

## Business rules
- BR-001: JobPosting and Application are separate; a job can be saved without applying.
- BR-002: One JobPosting can have zero or one Application in this single-user product.
- BR-003: Updating current status and creating RecruitmentEvent must be one database transaction.
- BR-004: Status values: WISHLIST, APPLIED, SCREENING, ASSESSMENT, INTERVIEW, OFFERING, ACCEPTED, REJECTED, WITHDRAWN. Flow is not strictly linear; transition validation lives in a service.
- BR-005: AI output is untrusted and validated against a strict schema.
- BR-006: AI may suggest evidence mapping; backend computes final score deterministically.
- BR-007: Evidence must point to profile/project/skill data; never invent evidence.
- BR-008: MATCH, PARTIAL_MATCH, GAP, UNKNOWN are distinct. UNKNOWN is not a GAP.
- BR-009: Each assessed criterion has positive weight and score 0–100. UNKNOWN has no score.
- BR-010: `fitScore = sum(weight * score) / sum(weight)` over assessed criteria only. `coverage = assessed applicable weight / total applicable weight * 100`. Display both together. If no criteria are assessed, fitScore is null.
- BR-011: Mandatory status is displayed but does not incur an extra penalty in v1. Any future penalty requires a spec change and scoring-version increment.
- BR-012: AI extraction creates a draft; it does not overwrite finalized data before user confirmation.
- BR-013: Analysis stores snapshots and version metadata so historical results remain interpretable.
- BR-014: Minimize personal data sent to the AI provider.
- BR-015: Documents are stored outside public web root and served only through authenticated endpoints.
- BR-016: URL importer allows only permitted HTTP(S) public content, uses SSRF controls, and never bypasses site access controls.
- BR-017: Delete operations must preserve referential integrity and should not silently erase application/analysis history.

## Non-functional requirements
- Passwords use a vetted adaptive hash; never plaintext.
- Session token is high-entropy; store only token hash in DB.
- Cookie HttpOnly, SameSite, Secure on HTTPS; state-changing requests protected against CSRF.
- Login rate limiting and generic authentication errors.
- Validate DTOs and external/AI data.
- No API keys in frontend or committed files.
- Pagination/filtering for lists.
- Database writes spanning status and event use transactions.
- Backup database and file volume together; test restore.
- Errors are understandable; UI does not rely only on color for status.

## MVP acceptance criteria
1. Login/logout and protected endpoints work.
2. Profile, skills, projects, jobs, and requirements persist.
3. Application status changes create matching events atomically.
4. Dashboard uses real DB data.
5. Score and coverage are both displayed; unknowns do not count as gaps.
6. AI failure does not corrupt saved data; manual flow still works.
7. Files cannot be fetched from public static paths and require authorization.
8. Lint, typecheck, relevant tests, and Prisma validation pass.
9. Backup and restore are tested.
