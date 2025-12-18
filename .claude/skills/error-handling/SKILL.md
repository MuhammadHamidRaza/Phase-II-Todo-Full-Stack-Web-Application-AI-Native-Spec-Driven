---
name: "error-handling"
description: "Handle errors in production-grade software systems with proper classification, context enrichment, consistent patterns, and observability for debugging"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade Error Handling Skill

## When to Use This Skill

Use this skill when designing or implementing error handling for:
- Production software systems requiring reliability
- Multi-service architectures where error propagation needs to be controlled
- Systems that require proper logging and monitoring
- Applications where user experience during errors is critical
- Services that interact with external dependencies

## Core Principles

1. **Error Classification**: Distinguish between client errors, server errors, infrastructure issues, and external service failures
2. **Context Enrichment**: Add relevant context (request IDs, user IDs, environment) to internal error representations
3. **Safe User Messages**: Provide minimal, safe, and actionable messages to end users
4. **Consistent Patterns**: Use consistent error handling patterns across all services
5. **Explicit Decision Making**: Make explicit decisions about retry, fallback, or fail-fast behavior
6. **Observability**: Design logs and metrics for debugging, not just visibility

## Process Steps

### 1. Error Classification
- Identify the type of error: client (4xx), server (5xx), infrastructure, or external service
- Determine if the error is recoverable or permanent
- Assess the scope of impact (single request, user, service, or system-wide)

### 2. Context Enrichment
- Add a unique request ID to trace the error through the system
- Include user ID if applicable (for user-specific debugging)
- Capture environment details (staging, production, etc.)
- Add relevant business context (operation being performed, affected resources)

### 3. User Message Generation
- Create a safe, non-technical message for end users
- Ensure the message doesn't leak system internals
- Provide actionable guidance when possible
- Use consistent language across the application

### 4. Internal Error Representation
- Create an enriched internal error object with context
- Include stack trace and technical details for developers
- Add appropriate HTTP status code or error code
- Capture metrics for monitoring and alerting

### 5. Decision Making
- Determine if the error should trigger a retry
- Identify if a fallback mechanism should be activated
- Decide whether to fail fast or continue with degraded functionality
- Evaluate circuit breaker or bulkhead patterns if applicable

### 6. Logging and Monitoring
- Log the internal error representation for debugging
- Ensure sensitive information is not logged
- Emit metrics for alerting and monitoring
- Track error patterns for system health analysis

## Error Classification Framework

### Client Errors (4xx)
- Invalid input parameters
- Authentication/authorization failures
- Resource not found
- Rate limiting

### Server Errors (5xx)
- Internal system failures
- Unhandled exceptions
- Database connection issues
- Third-party service failures

### Infrastructure Errors
- Network connectivity issues
- Resource exhaustion
- Platform-level failures

### External Service Errors
- Third-party API failures
- Integration timeouts
- Service unavailability

## Output Format

The skill produces:

1. **Error Classification**: Type of error with appropriate HTTP status code
2. **User-Facing Message**: Safe, minimal, actionable message
3. **Internal Error Object**: Enriched with context for debugging
4. **Action Decision**: Retry, fallback, or fail-fast strategy
5. **Logging Payload**: Structured data for logging and monitoring

## Example Implementation

### Input
```
User attempts to create a task but the database is temporarily unavailable
```

### Process
1. **Classification**: Server error (503 - Service Unavailable)
2. **Context Enrichment**: Add request ID, user ID, timestamp, operation context
3. **User Message**: "We're experiencing technical difficulties. Please try again in a moment."
4. **Internal Error**: Enriched error with stack trace, database connection details
5. **Decision**: Retry with exponential backoff
6. **Logging**: Structured log entry with context but no sensitive data

### Output
```json
{
  "userMessage": "We're experiencing technical difficulties. Please try again in a moment.",
  "httpStatusCode": 503,
  "internalError": {
    "id": "req-12345",
    "user": "user-67890",
    "timestamp": "2025-01-15T10:30:00Z",
    "operation": "create_task",
    "errorType": "DATABASE_UNAVAILABLE",
    "stackTrace": "...",
    "context": {
      "databaseHost": "db.example.com",
      "connectionTimeout": 5000
    }
  },
  "action": "retry",
  "retryPolicy": {
    "strategy": "exponential_backoff",
    "maxRetries": 3,
    "baseDelay": 1000
  },
  "logPayload": {
    "level": "error",
    "message": "Database connection failed during task creation",
    "requestId": "req-12345",
    "userId": "user-67890",
    "operation": "create_task",
    "errorType": "DATABASE_UNAVAILABLE",
    "timestamp": "2025-01-15T10:30:00Z"
  }
}
```

## Best Practices

- Always log internal errors but never expose internal details to users
- Use consistent error codes across your service ecosystem
- Implement circuit breakers for external dependencies
- Design for graceful degradation rather than complete failure
- Test error scenarios as part of your CI/CD pipeline
- Monitor error rates and patterns for system health
- Document error handling patterns for team consistency

## Anti-Patterns to Avoid

- Generic try-catch blocks without specific handling
- Exposing internal system details to end users
- Silent failures without logging or metrics
- Inconsistent error formats across services
- Not differentiating between recoverable and permanent errors
- Logging sensitive information in error messages