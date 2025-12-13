# Todo Full-Stack Web Application Constitution
<!-- Phase II — AI-Native, Spec-Driven Development -->

## Core Principles

### I. Spec-First Development
<!-- Every feature must have an approved specification before implementation -->
No feature or code without an approved specification. All development follows the Spec-Kit Plus methodology with formal specs, plans, and task breakdowns.

### II. AI-Driven Implementation
<!-- All code generated via Claude Code only -->
All code generation and implementation must be done through Claude Code. No manual code editing outside of Claude Code's guidance and tools.

### III. Consistency Across Stack
<!-- Frontend, backend, and database follow unified standards -->
Frontend, backend, and database layers must follow unified coding standards, naming conventions, and architectural patterns to ensure seamless integration.

### IV. Security by Default
<!-- Authentication and authorization enforced everywhere -->
Authentication and authorization mechanisms must be implemented and enforced at every layer of the application. No feature should be developed without considering security implications.

### V. Scalability & Maintainability
<!-- Code must support future phases (chatbot, agents) -->
All code must be written with future extensibility in mind, supporting upcoming phases such as chatbot and agent implementations. Follow clean architecture principles.

### VI. Reproducibility
<!-- Any developer or judge can run the project locally -->
The project must be completely reproducible in any environment. All dependencies, configurations, and setup procedures must be documented and automated.

## Technology Stack (Non-Negotiable)

### Frontend
- Framework: Next.js 16+
- Routing: App Router only
- Language: TypeScript
- Styling: Tailwind CSS
- Auth Integration: Better Auth (frontend SDK)
- API Access: Centralized API client (/lib/api.ts)

### Backend
- Framework: Python FastAPI
- Language: Python 3.10+
- API Style: REST (JSON only)
- Auth Verification: JWT verification middleware
- Config Management: Environment variables only

### Database & ORM
- Database: Neon Serverless PostgreSQL
- ORM: Prisma (TypeScript) for frontend, SQLAlchemy (Python) for backend

## Global Development Standards
- All implementation must strictly follow approved specs
- No direct code edits outside Claude Code
- No skipping Spec-Kit phases
- All changes must be traceable to a spec
- Frontend and backend changes must remain consistent

## Security Standards
- JWT token required for every API request
- Missing/invalid token → 401 Unauthorized
- User ID in JWT must match resource ownership
- All database queries must filter by authenticated user
- No cross-user data access under any condition

## API Quality Standards
- All routes under /api/
- RESTful naming conventions
- JSON request/response only
- Proper HTTP status codes:
  - 200 / 201 — Success
  - 400 — Bad request
  - 401 — Unauthorized
  - 403 — Forbidden
  - 404 — Not found
  - 422 — Validation error
  - 500 — Server error

## Data Standards
- Task entity must include:
  - id
  - user_id
  - title
  - description
  - completed
  - created_at
  - updated_at
- No orphaned tasks
- Data must persist across restarts
- Referential integrity enforced

## Constraints
- No alternative frameworks without spec update
- No hardcoded secrets
- No mock authentication
- No shared user sessions between services
- No undocumented behavior

## Success Criteria
- All 5 basic task features work end-to-end
- Multi-user isolation verified
- JWT enforced on every backend route
- Neon PostgreSQL stores persistent data
- Frontend is responsive on mobile & desktop
- Project passes Phase II evaluation checklist
- Clear AI-driven workflow visible in git history

## Governance
The constitution supersedes all other development practices. Any amendments require formal documentation, approval process, and migration plan if applicable.

All pull requests and reviews must verify compliance with these principles. Complexity must be justified with clear benefits. Use this document as the primary guidance for all development decisions.

**Version**: 1.0.0 | **Ratified**: 2025-12-13 | **Last Amended**: 2025-12-13
