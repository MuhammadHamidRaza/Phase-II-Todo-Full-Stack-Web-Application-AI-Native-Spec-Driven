# ADR-0004: Authentication Strategy

> **Scope**: Document decision clusters, not individual technology choices. Group related decisions that work together (e.g., "Frontend Stack" not separate ADRs for framework, styling, deployment).

- **Status:** Accepted
- **Date:** 2025-12-17
- **Feature:** Todo Full-Stack Web Application
- **Context:** Need to implement secure, scalable authentication that works across frontend and backend while ensuring user isolation. The solution must support JWT tokens for stateless authentication between services and integrate with the selected technology stack.

<!-- Significance checklist (ALL must be true to justify this ADR)
     1) Impact: Long-term consequence for architecture/platform/security?
     2) Alternatives: Multiple viable options considered with tradeoffs?
     3) Scope: Cross-cutting concern (not an isolated detail)?
     If any are false, prefer capturing as a PHR note instead of an ADR. -->

## Decision

- Authentication Provider: Better Auth
- Token Format: JWT (JSON Web Tokens)
- Token Flow: Better Auth issues JWTs on login, frontend attaches to API requests, backend verifies
- User Isolation: JWT verification middleware ensures user_id in token matches user_id in API routes
- Session Management: Stateless JWT tokens with configurable expiration
- Shared Secret: BETTER_AUTH_SECRET environment variable used by both frontend and backend
- Security Headers: Authorization: Bearer <token> for all authenticated requests

## Consequences

### Positive

- Statelessness - no need to maintain session state in backend
- Better Auth provides robust authentication with social login options
- JWT tokens can be verified independently by backend without calling frontend
- Clear separation between authentication provider and resource server
- Supports automatic token refresh mechanisms
- Industry-standard approach with good tooling support
- Enables microservice architecture by using self-contained tokens

### Negative

- JWT tokens cannot be revoked before expiration without additional infrastructure
- Larger payload size compared to session IDs
- Token expiration management complexity
- Shared secret management across services
- Need for additional security measures against token theft

## Alternatives Considered

Alternative A: Session-based authentication with shared Redis
- Why rejected: Creates stateful architecture, requires additional infrastructure, violates stateless principle

Alternative B: OAuth2 only (no Better Auth)
- Why rejected: Would limit authentication options, more complex setup for basic user authentication

Alternative C: Custom JWT implementation
- Why rejected: Better Auth provides more features and security out of the box

## References

- Feature Spec: specs/001-todo-full-stack/spec.md
- Implementation Plan: specs/001-todo-full-stack/plan.md
- Related ADRs: ADR-0001 (Frontend Technology Stack), ADR-0002 (Backend Technology Stack)
- Evaluator Evidence: research.md, data-model.md
