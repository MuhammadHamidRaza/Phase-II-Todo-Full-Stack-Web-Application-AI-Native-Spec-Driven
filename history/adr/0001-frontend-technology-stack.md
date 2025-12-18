# ADR-0001: Frontend Technology Stack

> **Scope**: Document decision clusters, not individual technology choices. Group related decisions that work together (e.g., "Frontend Stack" not separate ADRs for framework, styling, deployment).

- **Status:** Accepted
- **Date:** 2025-12-17
- **Feature:** Todo Full-Stack Web Application
- **Context:** Need to select a modern, scalable frontend stack that integrates well with the backend API and provides a responsive user experience across devices. The stack must support the project's requirements for authentication, task management, and rich UI interactions.

<!-- Significance checklist (ALL must be true to justify this ADR)
     1) Impact: Long-term consequence for architecture/platform/security?
     2) Alternatives: Multiple viable options considered with tradeoffs?
     3) Scope: Cross-cutting concern (not an isolated detail)?
     If any are false, prefer capturing as a PHR note instead of an ADR. -->

## Decision

- Framework: Next.js 16+ (App Router)
- Language: TypeScript
- Styling: Tailwind CSS
- Authentication: Better Auth frontend SDK
- API Access: Centralized API client (/lib/api.ts)
- Component Library: Custom components with Tailwind styling
- State Management: React Context API with custom hooks

## Consequences

### Positive

- Server-side rendering and static generation for better SEO and performance
- Strong TypeScript support with excellent type safety
- Built-in routing with App Router for modern navigation patterns
- Tailwind CSS provides utility-first approach for rapid UI development
- Better Auth integration simplifies authentication flow
- Strong ecosystem and community support
- Responsive design capabilities built into Tailwind
- Fast refresh during development for improved developer experience

### Negative

- Learning curve for developers unfamiliar with Next.js App Router
- Potential bundle size concerns with rich UI components
- Vendor lock-in to Next.js ecosystem
- Complexity of understanding server vs client components
- Additional complexity of managing authentication state across components

## Alternatives Considered

Alternative Stack A: React + Vite + Styled Components + Cloudflare Pages
- Why rejected: Less integrated solution, missing SSR benefits, more manual configuration required

Alternative Stack B: Vue.js 3 + Nuxt + Pinia + Vercel
- Why rejected: Would create inconsistency with project's Next.js requirement from constitution

Alternative Stack C: Angular + Material + Netlify
- Why rejected: Would violate constitution requirement for Next.js, steeper learning curve for team

## References

- Feature Spec: specs/001-todo-full-stack/spec.md
- Implementation Plan: specs/001-todo-full-stack/plan.md
- Related ADRs: ADR-0002 (Backend Technology Stack), ADR-0004 (Authentication Strategy)
- Evaluator Evidence: research.md, data-model.md
