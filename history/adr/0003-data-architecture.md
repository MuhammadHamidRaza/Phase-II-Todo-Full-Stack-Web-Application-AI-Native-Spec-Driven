# ADR-0003: Data Architecture

> **Scope**: Document decision clusters, not individual technology choices. Group related decisions that work together (e.g., "Frontend Stack" not separate ADRs for framework, styling, deployment).

- **Status:** Accepted
- **Date:** 2025-12-17
- **Feature:** Todo Full-Stack Web Application
- **Context:** Need to select a robust, scalable database solution that supports the application's requirements for multi-user isolation, data persistence, and efficient querying. The architecture must support the project's constraints for data standards and referential integrity.

<!-- Significance checklist (ALL must be true to justify this ADR)
     1) Impact: Long-term consequence for architecture/platform/security?
     2) Alternatives: Multiple viable options considered with tradeoffs?
     3) Scope: Cross-cutting concern (not an isolated detail)?
     If any are false, prefer capturing as a PHR note instead of an ADR. -->

## Decision

- Database: Neon Serverless PostgreSQL
- ORM: SQLModel for both frontend (Prisma-style) and backend (SQLAlchemy-style)
- Task Entity Structure:
  - id (Integer, Primary Key, Auto-increment)
  - user_id (String, Foreign Key to Better Auth user)
  - title (String, Not Null, max 200 chars)
  - description (Text, Nullable)
  - priority (String, values: 'low', 'medium', 'high', 'critical')
  - due_date (DateTime, Nullable)
  - completed (Boolean, default: false)
  - tags (Text Array for categorization)
  - created_at (DateTime, default: now)
  - updated_at (DateTime, default: now)
- Indexing Strategy: Indexes on user_id, completed, priority, and due_date
- Security: All queries must filter by authenticated user

## Consequences

### Positive

- PostgreSQL provides ACID compliance and strong data integrity
- Neon Serverless offers automatic scaling and reduced operational overhead
- SQLModel provides unified data models for both frontend and backend
- Robust querying capabilities with PostgreSQL's advanced features
- Strong security model with row-level security options
- Excellent performance with proper indexing strategy
- Support for complex queries needed for filtering and sorting
- Mature ecosystem with extensive tooling

### Negative

- Serverless PostgreSQL may have cold start latency issues
- More complex than document databases for simple key-value operations
- Potential cost considerations with serverless model at scale
- Requires more complex setup than simpler databases
- Learning curve for advanced PostgreSQL features

## Alternatives Considered

Alternative A: MongoDB + Mongoose
- Why rejected: Would violate constitution requirement for PostgreSQL, lacks strong referential integrity

Alternative B: SQLite + SQLAlchemy
- Why rejected: Would not scale well for multi-user application, lacks Neon's serverless benefits

Alternative C: Redis + PostgreSQL for caching
- Why rejected: Premature optimization, adds complexity without clear benefit at this scale

## References

- Feature Spec: specs/001-todo-full-stack/spec.md
- Implementation Plan: specs/001-todo-full-stack/plan.md
- Related ADRs: ADR-0002 (Backend Technology Stack), ADR-0004 (Authentication Strategy)
- Evaluator Evidence: research.md, data-model.md
