# Project Memory — Master Index
Last updated: 2025-04-06 | Session 15 | Branch: main
Memory health: 6/10 (frontend memory stale, API memory fresh)

## Project Overview
SaaS platform for project management ("TaskFlow"). Turborepo monorepo with
Next.js frontend, Express API, and shared packages. 4-person team. In active
development, beta launch completed, iterating toward GA.

## Domain Memory Files
| Domain | File | Status | Last Updated |
|--------|------|--------|-------------|
| Frontend | `MEMORY-frontend.md` | Stale (8 days) | Session 11 |
| API | `MEMORY-api.md` | Fresh | Session 15 |
| Shared | `MEMORY-shared.md` | Current | Session 14 |

**Current focus:** API team working on GraphQL migration (sessions 13-15).
Frontend paused while API stabilizes new schema.

## Cross-Domain Context

### Active Migrations
- REST → GraphQL migration in progress. API endpoints being converted
  incrementally. Frontend will switch after API layer is stable.
- Shared types package being restructured to export both REST and GraphQL types
  during transition. See `MEMORY-shared.md` for details.

### Cross-Domain Decisions
| Date | Decision | Reasoning | Affects |
|------|----------|-----------|---------|
| 2025-03-01 | Turborepo over Nx | Simpler config, Vercel-native support | All packages |
| 2025-03-15 | GraphQL over REST (migration) | Frontend needs flexible queries, REST over-fetching | API + Frontend |
| 2025-03-15 | Incremental migration | Can't afford full rewrite, REST and GraphQL coexist | All domains |
| 2025-04-01 | Shared Zod schemas | Single source of truth for validation across API + Frontend | Shared + API |

### Monorepo Structure
```
apps/
  web/              → Next.js 14 frontend (Vercel)
  api/              → Express + Apollo Server (Railway)
packages/
  shared/           → Types, Zod schemas, utils
  ui/               → Shared React components (Storybook)
  config/           → Shared ESLint, TypeScript, Tailwind config
```

## Where We Left Off
- **Current focus:** API — GraphQL schema for Task and Project entities
- **Frontend:** Paused at session 11, waiting for GraphQL API to stabilize
- **Shared:** Types package updated in session 14 to export GraphQL types
- **Next immediate step:** Complete Task mutations in `apps/api/src/graphql/resolvers/task.ts`, then wire up frontend with Apollo Client

## Blockers
- Frontend can't switch to GraphQL until at least Task + Project + User
  queries and mutations are all migrated. ETA: 2-3 more sessions.

## Session Log
| Session | Date | Domain | Summary |
|---------|------|--------|---------|
| 13 | 2025-04-01 | API | Apollo Server setup, User query + mutations |
| 14 | 2025-04-03 | Shared | GraphQL type exports, Zod schema alignment |
| 15 | 2025-04-06 | API | Project queries + mutations, started Task schema |
