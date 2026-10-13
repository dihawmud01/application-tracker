# Architecture and Data Model

## Architecture
Next.js frontend -> NestJS REST `/api/v1` -> domain modules -> Prisma -> PostgreSQL. NestJS alone accesses DB and AI provider. Documents use a persistent local volume outside public web root.

## Modules
Auth, Profile, Skills, Projects, Documents, Jobs, JobImport, Applications, Tasks, AIAnalysis, Dashboard, Prisma, Config.

## Rules
- Business rules live in NestJS services.
- DTOs validate API input; AI output is untrusted.
- Status update + recruitment event is transactional.
- AI provider is abstracted behind a backend adapter.
- URL import must block localhost/private/link-local/reserved IPs, validate redirects/DNS, restrict response size/time/content type, and not forward credentials.
- No microservices, queue, or Redis in MVP.

## ERD (conceptual)
```mermaid
erDiagram
  USER_ACCOUNT ||--o{ AUTH_SESSION : has
  CAREER_PROFILE ||--o{ EDUCATION : has
  CAREER_PROFILE ||--o{ EXPERIENCE : has
  CAREER_PROFILE ||--o{ CERTIFICATION : has
  CAREER_PROFILE ||--o{ PROFILE_SKILL : owns
  SKILL ||--o{ PROFILE_SKILL : maps
  CAREER_PROFILE ||--o{ PROJECT : owns
  PROJECT ||--o{ PROJECT_SKILL : demonstrates
  SKILL ||--o{ PROJECT_SKILL : maps
  CAREER_PROFILE ||--o{ DOCUMENT : owns
  DOCUMENT ||--o{ APPLICATION_DOCUMENT : attached
  APPLICATION ||--o{ APPLICATION_DOCUMENT : has
  JOB_POSTING ||--o{ JOB_REQUIREMENT : contains
  SKILL o|--o{ JOB_REQUIREMENT : maps
  JOB_POSTING ||--o{ IMPORT_ATTEMPT : attempts
  JOB_POSTING ||--o| APPLICATION : may_have
  APPLICATION ||--o{ RECRUITMENT_EVENT : records
  APPLICATION ||--o{ TASK : schedules
  CAREER_PROFILE ||--o{ AI_ANALYSIS : owns
  JOB_POSTING o|--o{ AI_ANALYSIS : evaluated
  AI_ANALYSIS ||--o{ AI_ANALYSIS_ITEM : contains
```

## Data model notes
- `Application.jobPostingId` unique: max one application per job.
- `ProfileSkill(profileId, skillId)` unique.
- `ProjectSkill(projectId, skillId)` composite primary key.
- JobRequirement may have nullable Skill mapping.
- `AIAnalysis.fitScore` nullable; coverage stored separately.
- `AIAnalysis.inputSnapshot` captures profile, job, and scoring configuration relevant to that analysis.
- Restrict deleting a job while an application/analysis depends on it; user must explicitly handle dependents.
- Files are stored in volume; DB stores metadata and generated storage key.
- Use Decimal for salary amounts; validate score and weight bounds in service.
