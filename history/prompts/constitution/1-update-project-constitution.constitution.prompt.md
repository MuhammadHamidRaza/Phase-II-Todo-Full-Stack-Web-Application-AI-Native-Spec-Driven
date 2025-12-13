---
id: 1
title: "Update Project Constitution"
stage: "constitution"
date: "2025-12-13"
surface: "agent"
model: "claude-sonnet-4-5-20250929"
feature: "none"
branch: "master"
user: "user"
command: "/sp.constitution"
labels: ["constitution", "setup", "spec-first"]
links:
  spec: null
  ticket: null
  adr: null
  pr: null
files:
  - ".specify/memory/constitution.md"
tests: []
---

# Update Project Constitution

## PROMPT_TEXT

```
/sp.constitution
Project: Phase II — Todo Full-Stack Web Application (AI-Native, Spec-Driven)

Core Principles

Spec-First Development — No feature or code without an approved specification

AI-Driven Implementation — All code generated via Claude Code only
[Request interrupted by user]Consistency Across Stack — Frontend, backend, and database follow unified standards

Security by Default — Authentication and authorization enforced everywhere

Scalability & Maintainability — Code must support future phases (chatbot, agents)

Reproducibility — Any developer or judge can run the project locally

Technology Stack (Non-Negotiable)
Frontend

Framework: Next.js 16+

Routing: App Router only

Language: TypeScript

Styling: Tailwind CSS

Auth Integration: Better Auth (frontend SDK)

API Access: Centralized API client (/lib/api.ts)

Backend
[Request interrupted by user]/sp.constitution
Project: Phase II — Todo Full-Stack Web Application (AI-Native, Spec-Driven)
Core Principles
Spec-First Development — No feature or code without an approved specification
AI-Driven Implementation — All code generated via Claude Code only
Consistency Across Stack — Frontend, backend, and database follow unified standards
Security by Default — Authentication and authorization enforced everywhere
Scalability & Maintainability — Code must support future phases (chatbot, agents)
Reproducibility — Any developer or judge can run the project locally
Technology Stack (Non-Negotiable)
Frontend
Framework: Next.js 16+
Routing: App Router only
Language: TypeScript
Styling: Tailwind CSS
Auth Integration: Better Auth (frontend SDK)
API Access: Centralized API client (/lib/api.ts)
Backend
Framework: Python FastAPI
Language: Python 3.10+
API Style: REST (JSON only)
Auth Verification: JWT verification middleware
Config Management: Environment variables only
Database & ORM
Database: Neon Serverless PostgreSQL
ORM:Global Development Standards
All implementation must strictly follow approved specs
No direct code edits outside Claude Code
No skipping Spec-Kit phases
All changes must be traceable to a spec
Frontend and backend changes must remain consistent
Security Standards
JWT token required for every API request
Missing/invalid token → 401 Unauthorized
User ID in JWT must match resource ownership
All database queries must filter by authenticated user
No cross-user data access under any condition
API Quality Standards
All routes under /api/
RESTful naming conventions
JSON request/response only
Proper HTTP status codes:
200 / 201 — Success
400 — Bad request
401 — Unauthorized
403 — Forbidden
404 — Not found
422 — Validation error
500 — Server error
Data Standards
Task entity must include:
id
user_id
title
description
completed
created_at
updated_at
No orphaned tasks
Data must persist across restarts
Referential integrity enforced
Constraints
No alternative frameworks without spec update
No hardcoded secrets
No mock authentication
No shared user sessions between services
No undocumented behavior
Success Criteria
All 5 basic task features work end-to-end
Multi-user isolation verified
JWT enforced on every backend route
Neon PostgreSQL stores persistent data
Frontend is responsive on mobile & desktop
Project passes Phase II evaluation checklist
Clear AI-driven workflow visible in git history
```

## RESPONSE_TEXT

Updated the project constitution file (.specify/memory/constitution.md) with the specified principles, technology stack, development standards, security requirements, API quality standards, data standards, constraints, and success criteria for the Todo Full-Stack Web Application.

## OUTCOME

The constitution file has been successfully updated with all the required specifications, including core principles (Spec-First Development, AI-Driven Implementation, Consistency Across Stack, Security by Default, Scalability & Maintainability, Reproducibility), technology stack requirements (Next.js 16+, Python FastAPI, Neon PostgreSQL), global development standards, security standards, API quality standards, data standards, constraints, and success criteria.

## EVALUATION

The constitution update was successful and aligns with all the requirements specified. The file now properly reflects the project's guiding principles and technical standards for the Todo Full-Stack Web Application.