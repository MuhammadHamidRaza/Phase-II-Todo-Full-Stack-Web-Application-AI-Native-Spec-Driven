# ADR-0002: Backend Technology Stack

> **Scope**: Document decision clusters, not individual technology choices. Group related decisions that work together (e.g., "Frontend Stack" not separate ADRs for framework, styling, deployment).

- **Status:** Accepted
- **Date:** 2025-12-17
- **Feature:** Todo Full-Stack Web Application
- **Context:** Need to select a robust, scalable backend stack that provides RESTful API services, integrates with the chosen database, and handles authentication/authorization. The stack must support the project's requirements for security, performance, and maintainability.

<!-- Significance checklist (ALL must be true to justify this ADR)
     1) Impact: Long-term consequence for architecture/platform/security?
     2) Alternatives: Multiple viable options considered with tradeoffs?
     3) Scope: Cross-cutting concern (not an isolated detail)?
     If any are false, prefer capturing as a PHR note instead of an ADR. -->

## Decision

- Framework: FastAPI
- Language: Python 3.10+
- Database ORM: SQLModel
- Authentication: JWT verification middleware
- API Style: REST with JSON responses
- Configuration: Environment variables
- Testing: pytest framework

## Consequences

### Positive

- FastAPI provides automatic API documentation (Swagger/OpenAPI)
- Excellent performance with async support
- Strong typing with Pydantic models for request/response validation
- Built-in support for modern Python features
- SQLModel provides unified data models for both SQLAlchemy and Pydantic
- JWT middleware ensures stateless, secure authentication
- Rich ecosystem of Python libraries for extended functionality
- Async capabilities support high concurrency

### Negative

- Python's Global Interpreter Lock (GIL) may limit CPU-bound operations
- Runtime type checking only (no compile-time safety like TypeScript)
- Dependency management can become complex in larger applications
- Memory usage may be higher than compiled languages
- Less control over low-level operations compared to compiled languages

## Alternatives Considered

Alternative Stack A: Node.js + Express + Prisma + Jest
- Why rejected: Would create inconsistency with Python requirement from constitution

Alternative Stack B: Django REST Framework + DRF + PostgreSQL
- Why rejected: Heavier framework than needed, less async support than FastAPI

Alternative Stack C: Flask + SQLAlchemy + Marshmallow + unittest
- Why rejected: More manual setup required compared to FastAPI's automatic features

## References

- Feature Spec: specs/001-todo-full-stack/spec.md
- Implementation Plan: specs/001-todo-full-stack/plan.md
- Related ADRs: ADR-0001 (Frontend Technology Stack), ADR-0003 (Data Architecture)
- Evaluator Evidence: research.md, data-model.md
