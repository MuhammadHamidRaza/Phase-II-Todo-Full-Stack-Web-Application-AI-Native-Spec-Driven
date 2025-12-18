# ADR-0005: API Architecture

> **Scope**: Document decision clusters, not individual technology choices. Group related decisions that work together (e.g., "Frontend Stack" not separate ADRs for framework, styling, deployment).

- **Status:** Accepted
- **Date:** 2025-12-17
- **Feature:** Todo Full-Stack Web Application
- **Context:** Need to design a RESTful API architecture that provides clean separation between frontend and backend, supports the required CRUD operations for task management, and enforces authentication and authorization. The API must follow consistent patterns and support the project's requirements for security and maintainability.

<!-- Significance checklist (ALL must be true to justify this ADR)
     1) Impact: Long-term consequence for architecture/platform/security?
     2) Alternatives: Multiple viable options considered with tradeoffs?
     3) Scope: Cross-cutting concern (not an isolated detail)?
     If any are false, prefer capturing as a PHR note instead of an ADR. -->

## Decision

- API Style: RESTful JSON API
- Base Path: All endpoints under /api/
- Endpoint Pattern: /api/{user_id}/tasks for task operations
- HTTP Methods: Standard REST methods (GET, POST, PUT, DELETE, PATCH)
- Response Format: JSON only
- Error Handling: Standard HTTP status codes (200/201 success, 400/401/403/404/500 for errors)
- Authentication: JWT token required in Authorization: Bearer header for all endpoints
- Request Validation: Pydantic models for request/response validation
- Rate Limiting: To be implemented as needed based on usage patterns

## Consequences

### Positive

- RESTful design provides familiar patterns for developers
- Standard HTTP status codes provide clear communication of request outcomes
- JSON format ensures compatibility across different clients
- Centralized authentication middleware ensures consistent security
- Pydantic validation provides strong type safety and automatic documentation
- Clear separation of concerns between frontend and backend
- Good tooling support for testing and documentation
- Scalable architecture that can support additional endpoints

### Negative

- RESTful approach may require more endpoints than GraphQL for complex queries
- Less flexibility in response shape compared to GraphQL
- Potential over-fetching of data in some scenarios
- Need to maintain consistent API versioning strategy as project evolves
- Authentication header requirement on every request adds slight overhead

## Alternatives Considered

Alternative A: GraphQL API with Apollo Server
- Why rejected: More complexity than needed for this use case, steeper learning curve

Alternative B: gRPC API
- Why rejected: Would not align with RESTful requirement from constitution, less web-friendly

Alternative C: WebSocket-based real-time API
- Why rejected: Would be overkill for basic task management, adds complexity without clear benefit

## References

- Feature Spec: specs/001-todo-full-stack/spec.md
- Implementation Plan: specs/001-todo-full-stack/plan.md
- Related ADRs: ADR-0002 (Backend Technology Stack), ADR-0004 (Authentication Strategy)
- Evaluator Evidence: research.md, data-model.md
