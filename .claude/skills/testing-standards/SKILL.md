---
name: "testing-standards"
description: "Implement comprehensive testing standards for full-stack applications including unit, integration, e2e, and performance testing with consistent patterns and quality metrics"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade Testing Standards Skill

## When to Use This Skill

Use this skill when establishing testing practices for:
- Full-stack applications requiring comprehensive test coverage
- Systems that need to maintain high reliability and quality standards
- Projects requiring CI/CD integration with automated testing
- Applications that must meet specific quality metrics and reliability targets
- Teams that need consistent testing patterns and methodologies
- Services that require performance and load testing validation

## Core Principles

1. **Test Pyramid**: Emphasize unit tests, moderate integration tests, and fewer end-to-end tests
2. **Quality First**: Maintain high test coverage and quality standards across all test types
3. **Automation**: Automate testing as much as possible in CI/CD pipelines
4. **Fast Feedback**: Provide quick feedback on test results to developers
5. **Maintainability**: Write tests that are easy to understand, maintain, and update
6. **Coverage**: Achieve meaningful test coverage that reflects actual application usage
7. **Reliability**: Ensure tests are deterministic and don't produce false positives

## Process Steps

### 1. Unit Testing Strategy
- Identify components and functions that need unit testing
- Write tests for individual functions and methods
- Mock external dependencies to isolate units
- Follow AAA pattern (Arrange, Act, Assert)
- Maintain high code coverage for critical business logic

### 2. Integration Testing Implementation
- Test interactions between different modules and services
- Validate database operations and API integrations
- Test authentication and authorization flows
- Verify external service integrations
- Include database transaction and connection tests

### 3. End-to-End Testing Setup
- Design user journey scenarios for critical paths
- Implement UI testing for frontend components
- Test complete API workflows from client to server
- Include cross-browser and mobile testing
- Validate security and performance in real scenarios

### 4. Performance and Load Testing
- Define performance benchmarks and SLAs
- Implement load testing for expected traffic patterns
- Test for peak usage scenarios and scaling
- Validate database query performance
- Monitor memory and resource usage under load

### 5. Test Organization and Structure
- Organize tests by feature, component, or module
- Use consistent naming conventions for tests
- Implement test fixtures and setup/teardown patterns
- Separate test environments from production
- Document test scenarios and expected outcomes

### 6. CI/CD Integration
- Integrate tests into continuous integration pipelines
- Set up automated test execution on code commits
- Implement test result reporting and monitoring
- Configure quality gates based on test results
- Set up test failure notifications and alerts

## Testing Framework Guidelines

### Unit Testing
- Focus on individual functions and methods
- Mock external dependencies
- Aim for 80%+ coverage of critical business logic
- Run tests quickly (under 10 seconds total)
- Use descriptive test names that explain expected behavior

### Integration Testing
- Test real interactions between components
- Use test databases and services
- Include database transaction tests
- Validate API endpoints and responses
- Test authentication and authorization flows

### End-to-End Testing
- Test complete user workflows
- Use real browsers and environments
- Include UI interaction tests
- Test cross-component functionality
- Validate business requirements fulfillment

## Output Format

The skill produces:

1. **Testing Strategy**: Complete testing approach for the application
2. **Test Organization**: Structure and naming conventions for tests
3. **Coverage Requirements**: Target metrics for different test types
4. **Testing Tools**: Recommended frameworks and libraries
5. **CI/CD Integration**: Pipeline configuration for automated testing
6. **Quality Gates**: Standards and metrics for test acceptance
7. **Reporting Setup**: Test result visualization and monitoring

## Example Implementation

### Input
```
Implement testing standards for a task management application with user authentication and CRUD operations
```

### Process
1. **Unit Tests**: Test individual functions (task validation, user authentication logic)
2. **Integration Tests**: Test API endpoints, database operations, authentication middleware
3. **E2E Tests**: Test complete user flows (login, create task, update task, delete task)
4. **Performance Tests**: Load test API endpoints and database queries
5. **Security Tests**: Test authentication and authorization for each endpoint
6. **CI/CD Integration**: Set up automated testing pipeline with quality gates

### Output
```json
{
  "testingStrategy": {
    "unit": {
      "coverageTarget": "80%",
      "framework": "Jest",
      "patterns": ["individual functions", "business logic", "validation"]
    },
    "integration": {
      "coverageTarget": "70%",
      "framework": "Jest with supertest",
      "patterns": ["API endpoints", "database operations", "auth flows"]
    },
    "e2e": {
      "coverageTarget": "90%",
      "framework": "Cypress",
      "patterns": ["user journeys", "UI interactions", "complete workflows"]
    },
    "performance": {
      "target": "response time < 500ms",
      "tool": "Artillery",
      "scenarios": ["concurrent users", "peak load", "database queries"]
    }
  },
  "organization": {
    "structure": "__tests__/[unit|integration|e2e]/[feature]",
    "naming": "describe('feature', () => { it('should behavior', () => { ... }) })",
    "fixtures": "separate fixture files for test data"
  },
  "ciCd": {
    "unit": "run on every commit",
    "integration": "run on PR to main",
    "e2e": "run on deployment to staging",
    "qualityGates": {
      "minCoverage": 80,
      "maxFlakeRate": 5,
      "maxExecutionTime": 300
    }
  }
}
```

## Best Practices

- Write tests that are fast, isolated, and deterministic
- Use descriptive test names that explain expected behavior
- Maintain a healthy test pyramid (many units, some integration, few e2e)
- Mock external dependencies in unit tests but test real integrations
- Run tests frequently and get quick feedback
- Maintain test data in a clean, reproducible state
- Document test scenarios and expected outcomes
- Regularly review and refactor tests for maintainability

## Anti-Patterns to Avoid

- Writing tests that depend on external services without mocking
- Creating tests that are too broad and test multiple things
- Having tests that are too slow to run in development
- Writing tests that break frequently due to implementation changes
- Testing implementation details instead of behavior
- Having tests that require manual setup or intervention
- Not maintaining test coverage metrics
- Writing tests that are difficult to understand or maintain