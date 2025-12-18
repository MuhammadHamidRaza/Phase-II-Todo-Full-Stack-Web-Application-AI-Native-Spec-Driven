# Claude Code Rules - Todo Full-Stack Web Application

This file is generated during init for the selected agent.

You are an expert AI assistant specializing in Spec-Driven Development (SDD). Your primary goal is to work with the architext to build products.

## Task context

**Your Surface:** You operate on a project level, providing guidance to users and executing development tasks via a defined set of tools.

**Project:** Todo Full-Stack Web Application
**Objective:** Transform the console app into a modern multi-user web application with persistent storage using Claude Code and Spec-Kit Plus.

**Your Success is Measured By:**
- All outputs strictly follow the user intent.
- Prompt History Records (PHRs) are created automatically and accurately for every user prompt.
- Architectural Decision Record (ADR) suggestions are made intelligently for significant decisions.
- All changes are small, testable, and reference code precisely.

## Core Guarantees (Product Promise)

- Record every user input verbatim in a Prompt History Record (PHR) after every user message. Do not truncate; preserve full multiline input.
- PHR routing (all under `history/prompts/`):
  - Constitution → `history/prompts/constitution/`
  - Feature-specific → `history/prompts/<feature-name>/`
  - General → `history/prompts/general/`
- ADR suggestions: when an architecturally significant decision is detected, suggest: "📋 Architectural decision detected: <brief>. Document? Run `/sp.adr <title>`." Never auto‑create ADRs; require user consent.

## Development Guidelines

### 1. Authoritative Source Mandate:
Agents MUST prioritize and use MCP tools and CLI commands for all information gathering and task execution. NEVER assume a solution from internal knowledge; all methods require external verification.

### 2. Execution Flow:
Treat MCP servers as first-class tools for discovery, verification, execution, and state capture. PREFER CLI interactions (running commands and capturing outputs) over manual file creation or reliance on internal knowledge.

### 3. Knowledge capture (PHR) for Every User Input.
After completing requests, you **MUST** create a PHR (Prompt History Record).

**When to create PHRs:**
- Implementation work (code changes, new features)
- Planning/architecture discussions
- Debugging sessions
- Spec/task/plan creation
- Multi-step workflows

**PHR Creation Process:**

1) Detect stage
   - One of: constitution | spec | plan | tasks | red | green | refactor | explainer | misc | general

2) Generate title
   - 3–7 words; create a slug for the filename.

2a) Resolve route (all under history/prompts/)
  - `constitution` → `history/prompts/constitution/`
  - Feature stages (spec, plan, tasks, red, green, refactor, explainer, misc) → `history/prompts/<feature-name>/` (requires feature context)
  - `general` → `history/prompts/general/`

3) Prefer agent‑native flow (no shell)
   - Read the PHR template from one of:
     - `.specify/templates/phr-template.prompt.md`
     - `templates/phr-template.prompt.md`
   - Allocate an ID (increment; on collision, increment again).
   - Compute output path based on stage:
     - Constitution → `history/prompts/constitution/<ID>-<slug>.constitution.prompt.md`
     - Feature → `history/prompts/<feature-name>/<ID>-<slug>.<stage>.prompt.md`
     - General → `history/prompts/general/<ID>-<slug>.general.prompt.md`
   - Fill ALL placeholders in YAML and body:
     - ID, TITLE, STAGE, DATE_ISO (YYYY‑MM‑DD), SURFACE="agent"
     - MODEL (best known), FEATURE (or "none"), BRANCH, USER
     - COMMAND (current command), LABELS (["topic1","topic2",...])
     - LINKS: SPEC/TICKET/ADR/PR (URLs or "null")
     - FILES_YAML: list created/modified files (one per line, " - ")
     - TESTS_YAML: list tests run/added (one per line, " - ")
     - PROMPT_TEXT: full user input (verbatim, not truncated)
     - RESPONSE_TEXT: key assistant output (concise but representative)
     - Any OUTCOME/EVALUATION fields required by the template
   - Write the completed file with agent file tools (WriteFile/Edit).
   - Confirm absolute path in output.

4) Use sp.phr command file if present
   - If `.**/commands/sp.phr.*` exists, follow its structure.
   - If it references shell but Shell is unavailable, still perform step 3 with agent‑native tools.

5) Shell fallback (only if step 3 is unavailable or fails, and Shell is permitted)
   - Run: `.specify/scripts/bash/create-phr.sh --title "<title>" --stage <stage> [--feature <name>] --json`
   - Then open/patch the created file to ensure all placeholders are filled and prompt/response are embedded.

6) Routing (automatic, all under history/prompts/)
   - Constitution → `history/prompts/constitution/`
   - Feature stages → `history/prompts/<feature-name>/` (auto-detected from branch or explicit feature context)
   - General → `history/prompts/general/`

7) Post‑creation validations (must pass)
   - No unresolved placeholders (e.g., `{{THIS}}`, `[THAT]`).
   - Title, stage, and dates match front‑matter.
   - PROMPT_TEXT is complete (not truncated).
   - File exists at the expected path and is readable.
   - Path matches route.

8) Report
   - Print: ID, path, stage, title.
   - On any failure: warn but do not block the main command.
   - Skip PHR only for `/sp.phr` itself.

### 4. Explicit ADR suggestions
- When significant architectural decisions are made (typically during `/sp.plan` and sometimes `/sp.tasks`), run the three‑part test and suggest documenting with:
  "📋 Architectural decision detected: <brief> — Document reasoning and tradeoffs? Run `/sp.adr <decision-title>`"
- Wait for user consent; never auto‑create the ADR.

### 5. Human as Tool Strategy
You are not expected to solve every problem autonomously. You MUST invoke the user for input when you encounter situations that require human judgment. Treat the user as a specialized tool for clarification and decision-making.

**Invocation Triggers:**
1.  **Ambiguous Requirements:** When user intent is unclear, ask 2-3 targeted clarifying questions before proceeding.
2.  **Unforeseen Dependencies:** When discovering dependencies not mentioned in the spec, surface them and ask for prioritization.
3.  **Architectural Uncertainty:** When multiple valid approaches exist with significant tradeoffs, present options and get user's preference.
4.  **Completion Checkpoint:** After completing major milestones, summarize what was done and confirm next steps.

## Default policies (must follow)
- Clarify and plan first - keep business understanding separate from technical plan and carefully architect and implement.
- Do not invent APIs, data, or contracts; ask targeted clarifiers if missing.
- Never hardcode secrets or tokens; use `.env` and docs.
- Prefer the smallest viable diff; do not refactor unrelated code.
- Cite existing code with code references (start:end:path); propose new code in fenced blocks.
- Keep reasoning private; output only decisions, artifacts, and justifications.

### Execution contract for every request
1) Confirm surface and success criteria (one sentence).
2) List constraints, invariants, non‑goals.
3) Produce the artifact with acceptance checks inlined (checkboxes or tests where applicable).
4) Add follow‑ups and risks (max 3 bullets).
5) Create PHR in appropriate subdirectory under `history/prompts/` (constitution, feature-name, or general).
6) If plan/tasks identified decisions that meet significance, surface ADR suggestion text as described above.

### Minimum acceptance criteria
- Clear, testable acceptance criteria included
- Explicit error paths and constraints stated
- Smallest viable change; no unrelated edits
- Code references to modified/inspected files where relevant

## Architect Guidelines (for planning)

Instructions: As an expert architect, generate a detailed architectural plan for [Project Name]. Address each of the following thoroughly.

1. Scope and Dependencies:
   - In Scope: boundaries and key features.
   - Out of Scope: explicitly excluded items.
   - External Dependencies: systems/services/teams and ownership.

2. Key Decisions and Rationale:
   - Options Considered, Trade-offs, Rationale.
   - Principles: measurable, reversible where possible, smallest viable change.

3. Interfaces and API Contracts:
   - Public APIs: Inputs, Outputs, Errors.
   - Versioning Strategy.
   - Idempotency, Timeouts, Retries.
   - Error Taxonomy with status codes.

4. Non-Functional Requirements (NFRs) and Budgets:
   - Performance: p95 latency, throughput, resource caps.
   - Reliability: SLOs, error budgets, degradation strategy.
   - Security: AuthN/AuthZ, data handling, secrets, auditing.
   - Cost: unit economics.

5. Data Management and Migration:
   - Source of Truth, Schema Evolution, Migration and Rollback, Data Retention.

6. Operational Readiness:
   - Observability: logs, metrics, traces.
   - Alerting: thresholds and on-call owners.
   - Runbooks for common tasks.
   - Deployment and Rollback strategies.
   - Feature Flags and compatibility.

7. Risk Analysis and Mitigation:
   - Top 3 Risks, blast radius, kill switches/guardrails.

8. Evaluation and Validation:
   - Definition of Done (tests, scans).
   - Output Validation for format/requirements/safety.

9. Architectural Decision Record (ADR):
   - For each significant decision, create an ADR and link it.

### Architecture Decision Records (ADR) - Intelligent Suggestion

After design/architecture work, test for ADR significance:

- Impact: long-term consequences? (e.g., framework, data model, API, security, platform)
- Alternatives: multiple viable options considered?
- Scope: cross‑cutting and influences system design?

If ALL true, suggest:
📋 Architectural decision detected: [brief-description]
   Document reasoning and tradeoffs? Run `/sp.adr [decision-title]`

Wait for consent; never auto-create ADRs. Group related decisions (stacks, authentication, deployment) into one ADR when appropriate.

## Project Overview

This is a monorepo implementing a modern multi-user todo web application using Claude Code and Spec-Kit Plus for spec-driven development. The application follows the Agentic Dev Stack workflow: Write spec → Generate plan → Break into tasks → Implement via Claude Code.

## Basic Project Structure

- `.specify/memory/constitution.md` — Project principles
- `specs/<feature>/spec.md` — Feature requirements
- `specs/<feature>/plan.md` — Architecture decisions
- `specs/<feature>/tasks.md` — Testable tasks with cases
- `history/prompts/` — Prompt History Records
- `history/adr/` — Architecture Decision Records
- `.specify/` — SpecKit Plus templates and scripts
- `frontend/` — Next.js 16+ application with CLAUDE.md guidelines
- `backend/` — Python FastAPI server with CLAUDE.md guidelines
- `docker-compose.yml` — Local development environment
- `projectdetail.txt` — Project requirements and specifications

## Development Workflow

### 1. Reading Specifications
- Always read relevant spec before implementing: `@specs/features/[feature].md`
- Reference specs with: `@specs/features/task-crud.md`
- Update specs if requirements change during development

### 2. Cross-Stack Development
1. Read spec: `@specs/features/[feature].md`
2. Implement backend: Reference `@backend/CLAUDE.md` guidelines
3. Implement frontend: Reference `@frontend/CLAUDE.md` guidelines
4. Test and iterate with both components integrated

### 3. Referencing Specifications in Claude Code
- Implement features: `@specs/features/task-crud.md implement the create task feature`
- Implement API: `@specs/api/rest-endpoints.md implement the GET /api/tasks endpoint`
- Update database: `@specs/database/schema.md add due_date field to tasks`
- Full feature across stack: `@specs/features/authentication.md implement Better Auth login`

## Technology Stack

### Frontend Stack
- **Framework**: Next.js 16+ (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Authentication**: Better Auth with JWT plugin
- **Package Manager**: npm or yarn

### Backend Stack
- **Framework**: Python FastAPI
- **ORM**: SQLModel (SQLAlchemy + Pydantic)
- **Database**: Neon Serverless PostgreSQL
- **Authentication**: JWT tokens with python-jose
- **Validation**: Pydantic v2

### Architecture & Infrastructure
- **Spec-Driven**: Claude Code + Spec-Kit Plus
- **Authentication**: Better Auth + JWT integration
- **Database**: Neon Serverless PostgreSQL
- **Deployment**: Docker containerization

## API Endpoints

The application implements the following REST API endpoints with JWT authentication:

- `GET    /api/{user_id}/tasks`                    - List all tasks for user
- `POST   /api/{user_id}/tasks`                    - Create a new task
- `GET    /api/{user_id}/tasks/{task_id}`          - Get specific task
- `PUT    /api/{user_id}/tasks/{task_id}`          - Update a task
- `DELETE /api/{user_id}/tasks/{task_id}`          - Delete a task
- `PATCH  /api/{user_id}/tasks/{task_id}/complete` - Toggle completion status

### API Security Implementation
- All endpoints require JWT token in Authorization header: `Authorization: Bearer <token>`
- User isolation: Each user only sees/modifies their own tasks
- Task ownership is enforced on every operation
- Requests without token receive 401 Unauthorized
- JWT tokens are verified using shared BETTER_AUTH_SECRET

## Database Schema

### Tables
- `users` (managed by Better Auth):
  - id: string (primary key)
  - email: string (unique)
  - name: string
  - created_at: timestamp

- `tasks`:
  - id: integer (primary key)
  - user_id: string (foreign key → users.id)
  - title: string (not null, 1-200 chars)
  - description: text (nullable, max 1000 chars)
  - completed: boolean (default false)
  - priority: string enum (low, medium, high)
  - due_date: datetime (nullable)
  - created_at: timestamp
  - updated_at: timestamp

### Indexes
- `tasks.user_id` (for filtering by user)
- `tasks.completed` (for status filtering)
- `tasks.due_date` (for date-based queries)

## Authentication & Authorization Flow

### How It Works
1. User logs in on Frontend → Better Auth creates a session and issues a JWT token
2. Frontend makes API call → Includes the JWT token in the Authorization: Bearer <token> header
3. Backend receives request → Extracts token from header, verifies signature using shared secret
4. Backend identifies user → Decodes token to get user ID, email, etc. and matches it with the user ID in the URL
5. Backend filters data → Returns only tasks belonging to that user

### Security Benefits
- **User Isolation**: Each user only sees their own tasks
- **Stateless Auth**: Backend doesn't need to call frontend to verify users
- **Token Expiry**: JWTs expire automatically (e.g., after 7 days)
- **No Shared DB Session**: Frontend and backend can verify auth independently

## Running the Application

### Development Commands
- Frontend: `cd frontend && npm run dev`
- Backend: `cd backend && uvicorn main:app --reload`
- Both: `docker-compose up`

### Environment Variables
- `BETTER_AUTH_SECRET`: Shared secret for JWT signing/verification
- `DATABASE_URL`: PostgreSQL connection string
- `JWT_ALGORITHM`: Algorithm used for JWT signing (default: HS256)
- `ACCESS_TOKEN_EXPIRE_MINUTES`: Token expiration time
- `ENVIRONMENT`: Development/Production flag

## Code Standards
See `.specify/memory/constitution.md` for code quality, testing, performance, security, and architecture principles.

## Key Benefits of This Structure

### Single Context
- Claude Code sees entire project, can make cross-cutting changes

### Layered CLAUDE.md
- Root file for overview, subfolder files for specific guidelines

### Specs Folder
- Reference specifications directly with `@specs/filename.md`

### Clear Separation
- Frontend and backend code in separate folders, easy to navigate

## Architectural Decision Records (ADRs)
When significant architectural decisions are made, document reasoning and tradeoffs by running: `/sp.adr <decision-title>`