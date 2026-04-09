# Project Memory
Last updated: 2025-04-05 | Session 12 | Branch: main
Memory health: 7/10 (2 stale file refs from infra work)

## Project Overview
Internal order management REST API for logistics company. Go 1.22, Chi router,
PostgreSQL, Redis for caching + rate limiting. Dockerized, deployed on AWS ECS.
API serves both the internal dashboard (React) and mobile app (Flutter).

## Where We Left Off
- **Current task:** Rate limiting middleware in `internal/middleware/ratelimit.go`
- **Status:** in progress — token bucket algorithm done, Redis backend next
- **Next immediate step:** [AK] Wire up Redis-backed counter in `ratelimit.go:47` replacing the in-memory map, then add per-endpoint config in `internal/config/ratelimit.yaml`
- **Open question:** Rate limit by API key or by IP? Leaning toward API key since all clients are authenticated.

## Completed
- [Sessions 1-4] Project setup, Docker config, CI pipeline, DB schema
- [2025-03-18] [JS] User and auth endpoints with JWT + refresh tokens
- [2025-03-20] [MR] Terraform for ECS, RDS, ElastiCache provisioning
- [2025-03-22] [JS] Order CRUD endpoints with pagination and filtering
- [2025-03-25] [AK] Input validation middleware using go-playground/validator
- [2025-03-28] [JS] Redis caching layer for GET endpoints (5min TTL)
- [2025-04-01] [JS] Order status webhooks to external systems
- [2025-04-03] [MR] Structured logging with slog, shipped to CloudWatch

## Active Work
- [ ] [AK] Rate limiting middleware — Redis backend (in progress)
- [ ] [JS] Order search with full-text PostgreSQL search (not started)
- [ ] [MR] Grafana dashboards for API latency + error rates (in progress)
- [ ] [AK] API documentation with Swagger/OpenAPI (not started)

## Blockers
- Rate limit thresholds need sign-off from product team. Using placeholder
  values (100 req/min) until confirmed. Not a hard blocker — can swap later.

## Key Decisions
| Date | Decision | Reasoning | Affects |
|------|----------|-----------|---------|
| 2025-03-10 | [JS] Chi over Gin | Better middleware composability, stdlib-aligned | All routing |
| 2025-03-10 | [JS] sqlc over GORM | Type-safe generated code, no reflection, explicit SQL | All DB queries |
| 2025-03-12 | [MR] ECS Fargate over EKS | Simpler ops for team size, lower cost at current scale | Deployment |
| 2025-03-15 | [JS] JWT with refresh rotation | Refresh tokens rotate on use, 15min access / 7day refresh | Auth |
| 2025-03-28 | [JS] Redis for caching + rate limiting | Already provisioned for caching, reuse for rate limiting | Middleware |
| 2025-04-03 | [MR] slog over zerolog | Stdlib, good enough for structured logging | All logging |

## Key Files
| File | Purpose | Last Modified |
|------|---------|---------------|
| `internal/middleware/ratelimit.go` | Rate limiting middleware | Session 12 |
| `internal/middleware/auth.go` | JWT validation + refresh | Session 6 |
| `internal/handlers/orders.go` | Order CRUD handlers | Session 9 |
| `internal/service/order_service.go` | Order business logic | Session 9 |
| `internal/repository/queries.sql` | sqlc query definitions | Session 10 |
| `internal/cache/redis.go` | Redis client + cache helpers | Session 10 |
| `docker-compose.yml` | Local dev stack (API, PG, Redis) | Session 8 |
| `terraform/main.tf` | AWS infrastructure | Session 11 |

## Architecture Notes
```
Client Request
  → Chi Router
    → Auth Middleware (JWT validation)
      → Rate Limit Middleware (token bucket)
        → Handler (input validation, call service)
          → Service (business logic)
            → Repository (sqlc-generated, hits PG or Redis cache)
```
- Middleware chain order matters: auth before rate limit (need API key for per-key limits).
- Repository layer checks Redis cache first, falls through to PostgreSQL.
- All writes invalidate relevant cache keys immediately.
- sqlc generates Go code from `queries.sql` — run `make generate` after SQL changes.

## Known Issues
- [JS] Refresh token rotation has a small race window under concurrent
  requests. Acceptable for internal API but needs fix before external access.
  Tracked in issue #34.
- [MR] ECS task health checks occasionally timeout during deploy. Added
  grace period but not root-caused yet.

## Session Log
| Session | Date | Summary |
|---------|------|---------|
| 10 | 2025-03-28 | [JS] Redis caching, [AK] started validation middleware |
| 11 | 2025-04-01 | [JS] Webhooks, [MR] Terraform updates for ElastiCache |
| 12 | 2025-04-05 | [AK] Rate limit algo, [MR] slog migration + CloudWatch |

## User Preferences
- [JS] Prefers explicit error returns over panics — always `if err != nil`
- [JS] Likes table-driven tests with descriptive subtest names
- [AK] Wants detailed code comments while learning the codebase
- [MR] Prefers Makefile targets over remembering long commands

## External Context
- PostgreSQL on RDS: connection string in `DATABASE_URL`
- Redis on ElastiCache: host in `REDIS_URL`
- AWS credentials: IAM roles in ECS, local dev uses `~/.aws/credentials`
- CI/CD: GitHub Actions → ECR push → ECS deploy (see `.github/workflows/deploy.yml`)
