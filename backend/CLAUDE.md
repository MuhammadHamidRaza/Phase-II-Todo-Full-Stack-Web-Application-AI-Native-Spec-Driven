# Backend Development Guidelines - Todo Full-Stack Web Application

## Project Context
You are working on the backend of a modern multi-user todo web application. This is a Python FastAPI application using SQLModel as the ORM, with Neon Serverless PostgreSQL as the database. The backend implements JWT-based authentication to work with Better Auth on the frontend and provides a secure REST API for the frontend application.

## Role & Responsibilities
As the backend developer, you are responsible for:
- Designing and implementing REST API endpoints following security best practices
- Creating database models using SQLModel with proper relationships and constraints
- Implementing JWT authentication middleware for secure API access
- Ensuring proper data validation and error handling
- Implementing user isolation to ensure data privacy between users
- Creating efficient database queries with proper indexing

## Technology Stack
- **Framework**: FastAPI
- **Language**: Python 3.9+
- **ORM**: SQLModel (SQLAlchemy + Pydantic)
- **Database**: Neon Serverless PostgreSQL
- **Authentication**: JWT tokens with python-jose
- **Validation**: Pydantic v2

## Project Structure
```
backend/
├── main.py                 # FastAPI application entry point
├── models.py               # SQLModel database models
├── schemas.py              # Pydantic request/response schemas
├── database.py             # Database connection and session management
├── auth.py                 # JWT authentication middleware and utilities
├── dependencies.py         # FastAPI dependency injection
├── routes/                 # API route handlers
│   ├── __init__.py
│   └── tasks.py           # Task-related endpoints
├── config.py              # Configuration settings
├── utils/                 # Utility functions
│   ├── security.py        # Security-related utilities
│   └── validators.py      # Data validation utilities
└── tests/                 # Backend tests
    ├── conftest.py
    ├── test_auth.py
    └── test_tasks.py
```

## Backend Architecture Guidelines

### 1. API Design Principles
- Follow RESTful API design conventions
- Use consistent naming patterns for endpoints
- Implement proper HTTP status codes (200, 201, 400, 401, 403, 404, 500)
- Design versioned APIs when necessary (e.g., /api/v1/)
- Implement proper request/response validation using Pydantic

### 2. Database Model Design
- Use SQLModel for all database interactions
- Implement proper relationships between models
- Add appropriate indexes for performance optimization
- Use UUIDs or auto-incrementing integers for primary keys
- Implement proper foreign key constraints
- Include created_at and updated_at timestamps on all models

### 3. Security Implementation
- Implement JWT-based authentication middleware
- Validate JWT tokens using proper secret keys
- Ensure user isolation by validating user_id in URL parameters
- Implement proper input validation to prevent injection attacks
- Use parameterized queries to prevent SQL injection
- Implement rate limiting for API endpoints where appropriate

### 4. Error Handling
- Use FastAPI's HTTPException for error responses
- Implement custom exception handlers for consistent error responses
- Log errors appropriately without exposing sensitive information
- Return meaningful error messages to clients
- Implement proper validation error handling

## API Endpoints Specification

### Authentication Endpoints (handled by Better Auth, but backend must verify)
- All endpoints require JWT token in Authorization header
- Token verification happens via middleware
- User identification from token payload

### Task Management Endpoints
```
GET    /api/{user_id}/tasks                    # List all tasks for user
POST   /api/{user_id}/tasks                    # Create a new task
GET    /api/{user_id}/tasks/{task_id}          # Get specific task
PUT    /api/{user_id}/tasks/{task_id}          # Update a task
DELETE /api/{user_id}/tasks/{task_id}          # Delete a task
PATCH  /api/{user_id}/tasks/{task_id}/complete # Toggle completion status
```

### Endpoint Implementation Guidelines
- Validate user_id in URL matches authenticated user
- Implement proper request/response models using Pydantic
- Include pagination for list endpoints
- Implement filtering, sorting, and search capabilities
- Return appropriate HTTP status codes
- Include proper response validation

## Database Models

### User Model (managed by Better Auth)
- ID, email, name, created_at (provided by Better Auth)
- Backend only needs to reference user_id for task association

### Task Model
- id: Primary key (auto-incrementing integer)
- user_id: Foreign key referencing user (string from Better Auth)
- title: String (required, 1-200 characters)
- description: Text (optional, max 1000 characters)
- completed: Boolean (default: False)
- priority: String enum (low, medium, high)
- due_date: DateTime (optional)
- created_at: Timestamp (auto-generated)
- updated_at: Timestamp (auto-updated)

### Model Implementation Guidelines
- Use SQLModel's SQLModel and Field classes
- Implement proper validation constraints
- Add appropriate indexes for query performance
- Include proper relationship definitions where needed
- Implement proper serialization options

## Authentication & Authorization

### JWT Implementation
- Implement JWT middleware to verify tokens
- Extract user information from JWT payload
- Validate token signature using shared secret
- Handle token expiration properly
- Return 401 Unauthorized for invalid tokens

### User Isolation
- Validate user_id in URL parameter matches authenticated user
- Filter all database queries by user_id
- Prevent users from accessing other users' data
- Implement proper error handling for unauthorized access

### Security Headers
- Implement proper CORS configuration
- Add security headers to responses
- Validate Content-Type headers for POST/PUT requests
- Implement proper CSRF protection where necessary

## Request/Response Validation

### Request Models (Pydantic)
- Create separate models for each request type
- Implement proper field validation
- Use type hints for better IDE support
- Include example values where helpful
- Implement custom validators when necessary

### Response Models (Pydantic)
- Create separate models for each response type
- Exclude sensitive information from responses
- Implement proper serialization rules
- Include only necessary fields in responses
- Use response_model parameters in FastAPI routes

## Database Operations

### Session Management
- Use dependency injection for database sessions
- Implement proper session lifecycle management
- Handle transactions appropriately
- Close sessions properly after requests
- Implement connection pooling for performance

### Query Optimization
- Use proper indexing for frequently queried fields
- Implement pagination for large datasets
- Use eager loading where appropriate to prevent N+1 queries
- Optimize queries for performance
- Use database transactions for data consistency

## Environment Configuration

### Required Environment Variables
- DATABASE_URL: PostgreSQL connection string
- BETTER_AUTH_SECRET: Shared secret for JWT verification
- JWT_ALGORITHM: Algorithm used for JWT signing (default: HS256)
- ACCESS_TOKEN_EXPIRE_MINUTES: Token expiration time
- ENVIRONMENT: Development/Production flag

### Configuration Management
- Use Pydantic's BaseSettings for configuration
- Implement proper environment-specific settings
- Validate required environment variables
- Provide default values where appropriate
- Store sensitive information in environment variables only

## Testing Guidelines

### Unit Testing
- Write unit tests for utility functions
- Test model validation and constraints
- Test authentication middleware logic
- Use pytest for test execution
- Implement proper test fixtures

### Integration Testing
- Test API endpoints with different scenarios
- Test authentication and authorization flows
- Test database operations with real database
- Test error handling scenarios
- Implement comprehensive test coverage

### Database Testing
- Use separate test database
- Implement proper test data setup and teardown
- Test database constraints and relationships
- Test query performance
- Test transaction handling

## Performance Optimization

### Database Performance
- Implement proper database indexing
- Optimize queries for performance
- Use connection pooling
- Implement caching where appropriate
- Monitor query performance

### API Performance
- Implement proper request/response caching
- Optimize serialization/deserialization
- Use asynchronous operations where appropriate
- Implement proper pagination
- Monitor API response times

## Error Handling & Logging

### Error Response Format
```json
{
  "detail": "Error message",
  "error_code": "ERROR_CODE",
  "timestamp": "ISO 8601 timestamp"
}
```

### Logging Implementation
- Use Python's logging module
- Log appropriate information without sensitive data
- Implement structured logging
- Configure different log levels for different environments
- Include request IDs for tracing

## Deployment Considerations
- Use environment variables for configuration
- Implement proper health check endpoints
- Configure proper process management
- Set up monitoring and alerting
- Implement proper backup strategies
- Ensure database migration strategies