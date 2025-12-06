# Project Rules for TaskMaster Pro

## Development Standards

### Code Quality
- All code must pass linting (ESLint for TS, Black for Python)
- Minimum 80% test coverage
- No commented-out code in production
- Use TypeScript strict mode

### Git Workflow (SSCS)
- Branch naming: `feature/TM-{issue}-{description}`
- Commits: `feat/fix/docs/test: TM-{issue} - {description}`
- Pull requests require 1 approval
- No direct commits to main

### Testing Requirements
- Unit tests for all business logic
- Integration tests for API endpoints
- E2E tests for critical user flows
- Test data must be isolated

### Security
- Never commit secrets or API keys
- All user inputs must be sanitized
- Passwords must be hashed with bcrypt
- API endpoints must have rate limiting

### Documentation
- README for setup instructions
- API endpoints documented in OpenAPI
- Component storybook for UI elements
- Architecture decision records (ADRs)

## ZeroDB Schema Rules

### Naming Conventions
- Table names: snake_case, plural (e.g., `users`, `task_assignments`)
- Field names: snake_case
- Indexes: `idx_{table}_{field}`
- Foreign keys: `fk_{table}_{ref_table}`

### Data Modeling
- Use UUIDs for primary keys
- Created_at and updated_at for all tables
- Soft deletes with is_deleted flag
- Use JSONB for flexible metadata

### Vector Collections
- task_embeddings: For task similarity search
- comment_embeddings: For semantic comment search
- Use pgvector with cosine similarity

### Memory Tables
- active_sessions: User session data (TTL: 24h)
- rate_limits: API rate limiting (TTL: 1h)
- real_time_updates: WebSocket message queue (TTL: 5m)

## API Design

### REST Conventions
- GET /api/v1/tasks - List tasks
- POST /api/v1/tasks - Create task
- GET /api/v1/tasks/{id} - Get task
- PUT /api/v1/tasks/{id} - Update task
- DELETE /api/v1/tasks/{id} - Delete task

### Error Handling
- Use HTTP status codes correctly
- Return structured error responses
- Log all errors with context
- Never expose internal errors to users

### Pagination
- Use cursor-based pagination for large datasets
- Default page size: 20 items
- Max page size: 100 items

## Deployment

### Environments
- Development: Local Docker Compose
- Staging: Railway (auto-deploy from develop)
- Production: Railway (manual deploy from main)

### Database Migrations
- Use Alembic for migrations
- Test migrations on staging first
- Never delete columns (deprecate instead)
- Backup before migrations

### Monitoring
- Log levels: DEBUG (dev), INFO (staging), WARNING (prod)
- APM: Sentry for error tracking
- Metrics: Prometheus + Grafana
- Uptime: Better Uptime monitoring

## Definition of Done

A task is only complete when:
- ✅ Code is written and passes all tests
- ✅ PR is reviewed and approved
- ✅ Documentation is updated
- ✅ Deployed to staging and verified
- ✅ Product owner accepts the feature
