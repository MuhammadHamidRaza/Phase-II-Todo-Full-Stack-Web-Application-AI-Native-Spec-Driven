# Frontend Development Guidelines - Todo Full-Stack Web Application

## Project Context
You are working on the frontend of a modern multi-user todo web application. This is a Next.js 16+ application using the App Router, TypeScript, and Tailwind CSS. The application integrates with Better Auth for authentication and communicates with a FastAPI backend.

## Role & Responsibilities
As the frontend developer, you are responsible for:
- Creating responsive, accessible UI components using Next.js and Tailwind CSS
- Implementing authentication flows with Better Auth
- Creating API clients to communicate with the backend
- Ensuring proper state management and user experience
- Implementing responsive design for all screen sizes

## Technology Stack
- **Framework**: Next.js 16+ (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Authentication**: Better Auth with JWT plugin
- **Package Manager**: npm or yarn

## Project Structure
```
frontend/
├── app/                    # Next.js App Router pages
│   ├── login/page.tsx      # Login page
│   ├── register/page.tsx   # Registration page
│   ├── dashboard/page.tsx  # Main dashboard
│   └── layout.tsx          # Root layout
├── components/            # Reusable UI components
│   ├── TaskForm.tsx       # Task creation/editing form
│   ├── TaskItem.tsx       # Individual task display
│   ├── TaskList.tsx       # Task listing component
│   ├── TaskFilters.tsx    # Task filtering component
│   └── AuthProvider.tsx   # Authentication context provider
├── lib/                   # Utilities and API clients
│   ├── api.ts            # API client for backend communication
│   ├── auth.ts           # Authentication utilities
│   └── types.ts          # TypeScript type definitions
├── hooks/                 # Custom React hooks
│   └── useAuth.ts        # Authentication hook
├── styles/                # Global styles
└── public/                # Static assets
```

## Frontend Architecture Guidelines

### 1. Component Architecture
- Use server components by default for data fetching and rendering
- Use client components only when interactivity is required (use 'use client' directive)
- Create reusable, composable components in the `/components` directory
- Follow atomic design principles where appropriate
- Implement proper TypeScript interfaces for all props

### 2. State Management
- Use React Context for global state (authentication, user data)
- Use React hooks (useState, useEffect, useReducer) for component-level state
- Implement proper loading and error states for all async operations
- Use SWR or React Query for server state management and caching

### 3. Styling Guidelines
- Use Tailwind CSS utility classes exclusively
- No inline styles or custom CSS files
- Follow consistent design patterns and spacing (use Tailwind's spacing scale)
- Implement responsive design using Tailwind's responsive prefixes
- Use Tailwind's dark mode support where applicable

### 4. Authentication Implementation
- Integrate Better Auth with JWT plugin for authentication
- Create AuthProvider component to manage authentication state
- Implement useAuth custom hook for authentication context
- Ensure all API calls include JWT tokens in Authorization header
- Handle authentication errors and token expiration gracefully

## API Integration

### 1. API Client Structure
```typescript
// lib/api.ts
interface ApiClient {
  getTasks: (user_id: string) => Promise<Task[]>;
  createTask: (user_id: string, task: TaskCreateData) => Promise<Task>;
  updateTask: (user_id: string, task_id: number, task: TaskUpdateData) => Promise<Task>;
  deleteTask: (user_id: string, task_id: number) => Promise<void>;
  toggleTaskCompletion: (user_id: string, task_id: number) => Promise<Task>;
}
```

### 2. API Call Implementation
- All backend communication through centralized API client
- Include JWT token in Authorization header for authenticated requests
- Implement proper error handling and user feedback
- Use TypeScript interfaces for request/response types
- Implement proper loading states during API operations

## User Interface Guidelines

### 1. Task Management UI
- Create TaskForm component for creating and editing tasks
- Create TaskItem component for displaying individual tasks with completion toggle
- Create TaskList component for displaying multiple tasks
- Implement TaskFilters component for filtering and sorting tasks
- Ensure all components are accessible (ARIA labels, keyboard navigation)

### 2. Authentication UI
- Create login page with form validation
- Create registration page with form validation
- Implement proper error messaging for auth failures
- Create protected routes for authenticated users only
- Implement logout functionality

### 3. Responsive Design
- Implement mobile-first responsive design
- Use Tailwind's responsive prefixes (sm:, md:, lg:, xl:, 2xl:)
- Ensure touch targets are appropriately sized for mobile
- Optimize forms for mobile input
- Implement proper navigation for mobile users

## Error Handling & UX

### 1. Error States
- Implement error boundaries for handling component-level errors
- Show user-friendly error messages for API failures
- Implement proper form validation with clear error messages
- Handle network failures gracefully with retry mechanisms
- Implement loading states for all async operations

### 2. User Experience
- Provide immediate feedback for user actions
- Implement optimistic updates where appropriate
- Use appropriate loading indicators
- Ensure fast initial page loads
- Implement proper form validation and error messaging

## Testing Guidelines
- Write unit tests for utility functions and hooks
- Implement integration tests for API client
- Use component testing for UI components
- Test authentication flows thoroughly
- Test responsive behavior across different screen sizes

## Security Considerations
- Never expose JWT tokens in client-side logs
- Implement proper input validation on forms
- Sanitize user input before sending to backend
- Ensure authentication state is properly managed
- Implement CSRF protection where necessary

## Performance Optimization
- Use Next.js Image component for optimized images
- Implement proper code splitting with dynamic imports
- Optimize API calls with caching strategies
- Minimize bundle size through proper imports
- Implement lazy loading for non-critical components

## Environment Variables
- Store API endpoints in environment variables
- Configure Better Auth with proper environment settings
- Never hardcode sensitive information in source code
- Use .env.local for local development settings

## Deployment Considerations
- Optimize for production builds with `next build`
- Configure proper environment variables for different environments
- Implement proper error logging and monitoring
- Set up proper caching headers
- Ensure proper security headers are in place