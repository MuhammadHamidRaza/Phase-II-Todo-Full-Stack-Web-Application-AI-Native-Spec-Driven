---
name: "api-design"
description: "Design production-grade RESTful APIs with consistent structure, HTTP methods, authentication, error handling, and maintainability"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade API Design Skill

## When to Use This Skill

Use this skill when designing or implementing backend APIs that require:
- Consistent and intuitive resource structure
- Standardized HTTP methods and response formats
- Proper authentication and authorization
- Meaningful error handling and status codes
- Well-documented endpoints
- Testable and maintainable patterns
- Scalability and performance considerations

## Core Principles

1. **Resource-Centric Design**: Structure APIs around resources with clear, predictable endpoints
2. **HTTP Method Consistency**: Use standard HTTP methods appropriately (GET, POST, PUT, PATCH, DELETE)
3. **Standardized Formats**: Maintain consistent request and response formats across all endpoints
4. **Security First**: Implement proper authentication and authorization at every endpoint
5. **Meaningful Errors**: Provide clear error messages with appropriate HTTP status codes
6. **Documentation**: Document all endpoints, parameters, and expected responses
7. **Maintainability**: Design APIs that are easy to test, extend, and maintain

## Process Steps

### 1. Resource and Endpoint Structure Design
- Identify core resources in the system (users, tasks, orders, etc.)
- Design RESTful URL patterns using nouns (not verbs)
- Establish consistent naming conventions (plural nouns, kebab-case for specific items)
- Plan nested resources when relationships exist (e.g., /users/{id}/tasks)

### 2. HTTP Method Mapping
- GET: Retrieve resources (safe, idempotent)
- POST: Create new resources or trigger actions
- PUT: Update entire resources (idempotent)
- PATCH: Partial resource updates
- DELETE: Remove resources (idempotent)

### 3. Request and Response Standardization
- Define standard request body formats
- Establish consistent response structure with metadata (pagination, etc.)
- Use appropriate content types (application/json)
- Include standard headers (Content-Type, Authorization, etc.)

### 4. Authentication and Authorization
- Implement standard authentication methods (JWT, OAuth, API keys)
- Design role-based access control (RBAC) patterns
- Ensure authorization checks at every endpoint
- Secure sensitive operations with additional verification

### 5. Error Handling and Status Codes
- Use standard HTTP status codes appropriately
- Design consistent error response format
- Differentiate between client and server errors
- Log errors appropriately without exposing internals

### 6. Documentation and Testing
- Document all endpoints with examples
- Specify request parameters, headers, and body schema
- Define expected response formats and error cases
- Create test patterns for each endpoint type

## Resource Structure Guidelines

### URL Patterns
- Use plural nouns: `/users`, `/tasks`, `/orders`
- Use kebab-case for specific items: `/users/user-profile`, `/tasks/daily-tasks`
- Nest related resources: `/users/{id}/tasks`, `/orders/{id}/items`
- Use query parameters for filtering, sorting, pagination: `?status=active&page=1&limit=10`

### Standard Endpoints
```
GET    /resources          # List resources
POST   /resources          # Create resource
GET    /resources/{id}     # Get specific resource
PUT    /resources/{id}     # Update entire resource
PATCH  /resources/{id}     # Partial update
DELETE /resources/{id}     # Delete resource
```

## HTTP Status Code Guidelines

### Success Codes
- 200: OK (GET, PUT, PATCH)
- 201: Created (POST)
- 204: No Content (DELETE, sometimes PUT/PATCH)

### Client Error Codes
- 400: Bad Request (invalid input)
- 401: Unauthorized (missing/invalid credentials)
- 403: Forbidden (insufficient permissions)
- 404: Not Found (resource doesn't exist)
- 409: Conflict (resource already exists, etc.)
- 422: Unprocessable Entity (validation errors)

### Server Error Codes
- 500: Internal Server Error
- 502: Bad Gateway (when proxying to other services)
- 503: Service Unavailable (temporary unavailability)

## Standard Request/Response Formats

### Request Format
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "metadata": {
    "source": "web",
    "version": "1.0"
  }
}
```

### Success Response Format
```json
{
  "data": {
    "id": "123",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2025-01-15T10:30:00Z"
  },
  "meta": {
    "timestamp": "2025-01-15T10:30:00Z",
    "requestId": "req-12345"
  }
}
```

### Error Response Format
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input parameters",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address"
      }
    ]
  },
  "meta": {
    "timestamp": "2025-01-15T10:30:00Z",
    "requestId": "req-12345"
  }
}
```

## Authentication and Authorization Patterns

### Token-Based Authentication
- Use JWT tokens in Authorization header: `Authorization: Bearer {token}`
- Include token refresh mechanisms
- Validate token expiration and signature
- Include user roles/scopes in token payload

### Role-Based Access Control
- Define roles and permissions clearly
- Check permissions at the beginning of sensitive endpoints
- Return 403 for unauthorized access attempts
- Use middleware for common authorization checks

## Output Format

The skill produces:

1. **API Structure**: Resource-based URL patterns and endpoint designs
2. **HTTP Method Mappings**: Appropriate method usage for each endpoint
3. **Request/Response Schemas**: Standardized formats for all interactions
4. **Security Implementation**: Authentication and authorization patterns
5. **Error Handling Strategy**: Consistent error responses and status codes
6. **Documentation Template**: Standardized endpoint documentation
7. **Testing Strategy**: Patterns for testing each endpoint type

## Example Implementation

### Input
```
Design an API for a task management system with users and tasks
```

### Process
1. **Resources Identified**: users, tasks
2. **URL Structure**: /users, /users/{id}, /users/{id}/tasks, /tasks, /tasks/{id}
3. **HTTP Methods**: Standard CRUD operations mapped appropriately
4. **Authentication**: JWT token required for all endpoints
5. **Authorization**: Users can only access their own tasks
6. **Error Handling**: Standardized error responses with appropriate status codes

### Output
```
# Task Management API

## Authentication
All endpoints require a valid JWT token in the Authorization header:
`Authorization: Bearer {token}`

## Endpoints

### Users
- GET    /api/users          # List users (admin only)
- POST   /api/users          # Create user
- GET    /api/users/{id}     # Get user profile
- PUT    /api/users/{id}     # Update user profile
- DELETE /api/users/{id}     # Delete user (admin only)

### Tasks
- GET    /api/tasks                    # List user's tasks
- POST   /api/tasks                    # Create task
- GET    /api/tasks/{id}               # Get specific task
- PUT    /api/tasks/{id}               # Update entire task
- PATCH  /api/tasks/{id}               # Partial task update
- DELETE /api/tasks/{id}               # Delete task

## Response Format
{
  "data": { ... },
  "meta": {
    "timestamp": "2025-01-15T10:30:00Z",
    "requestId": "req-12345"
  }
}

## Error Format
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Error message",
    "details": [ ... ]
  },
  "meta": {
    "timestamp": "2025-01-15T10:30:00Z",
    "requestId": "req-12345"
  }
}
```

## Best Practices

- Use nouns, not verbs in endpoint URLs
- Use plural nouns for collections
- Use kebab-case for specific resource identifiers
- Implement proper pagination for collection endpoints
- Use appropriate HTTP status codes consistently
- Validate input data and return clear error messages
- Implement rate limiting for public endpoints
- Use versioning (e.g., /api/v1/) for API evolution
- Design for idempotency where possible
- Implement proper caching headers when appropriate

## Anti-Patterns to Avoid

- Using verbs in URLs (e.g., /getUsers, /deleteTask)
- Inconsistent HTTP method usage
- Exposing internal database IDs directly
- Returning different response formats from similar endpoints
- Missing authentication/authorization checks
- Using 200 status code for all responses including errors
- Not validating input parameters
- Returning sensitive information in error messages