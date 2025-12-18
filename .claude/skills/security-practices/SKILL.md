---
name: "security-practices"
description: "Implement comprehensive security practices for full-stack applications including authentication, authorization, input validation, and secure coding patterns"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade Security Practices Skill

## When to Use This Skill

Use this skill when implementing security measures for:
- Full-stack web applications requiring authentication and authorization
- Systems handling sensitive user data or financial transactions
- Applications that need to comply with security standards (OWASP, PCI, etc.)
- Services that interact with external APIs or third-party systems
- Applications requiring secure data storage and transmission
- Systems that need to protect against common web vulnerabilities

## Core Principles

1. **Defense in Depth**: Implement multiple layers of security controls
2. **Least Privilege**: Grant minimum necessary permissions to users and services
3. **Secure by Default**: Security should be enabled by default, not optional
4. **Zero Trust**: Never trust, always verify - validate all inputs and authenticate all requests
5. **Security First**: Consider security implications at every stage of development
6. **Regular Audits**: Continuously monitor and review security measures
7. **Incident Response**: Plan for security breaches and have response procedures

## Process Steps

### 1. Authentication Implementation
- Choose appropriate authentication method (JWT, OAuth, Session-based)
- Implement secure password handling (bcrypt, scrypt, argon2)
- Add multi-factor authentication (MFA) where appropriate
- Design secure login/logout flows
- Implement account lockout mechanisms after failed attempts

### 2. Authorization and Access Control
- Implement role-based access control (RBAC) or attribute-based access control (ABAC)
- Design permission hierarchies and inheritance patterns
- Create middleware for authorization checks
- Implement resource-level access controls
- Regular access reviews and permission audits

### 3. Input Validation and Sanitization
- Validate all user inputs on both client and server side
- Implement strict schema validation for API requests
- Sanitize inputs to prevent injection attacks (SQL, XSS, etc.)
- Use allowlists rather than blocklists for validation
- Implement rate limiting to prevent abuse

### 4. Data Protection
- Encrypt sensitive data at rest and in transit
- Use proper key management and rotation strategies
- Implement data classification and handling procedures
- Mask sensitive data in logs and error messages
- Apply data retention and deletion policies

### 5. Secure Communication
- Enforce HTTPS with proper TLS configuration
- Implement HSTS headers to prevent downgrade attacks
- Use secure cookies with appropriate flags (HttpOnly, Secure, SameSite)
- Apply CORS policies appropriately
- Implement proper certificate management

### 6. Security Monitoring and Logging
- Log security-relevant events without exposing sensitive data
- Implement intrusion detection patterns
- Monitor for suspicious activities and anomalies
- Set up security alerts and incident response procedures
- Regular security audits and penetration testing

## Security Implementation Framework

### Authentication Patterns
- Password-based: Secure hashing with salt, strength requirements
- Token-based: JWT with proper expiration and refresh mechanisms
- OAuth: Third-party authentication with proper scopes
- API Keys: For service-to-service communication with rotation

### Authorization Models
- Role-Based Access Control (RBAC): User roles and permissions
- Attribute-Based Access Control (ABAC): Context-aware permissions
- Resource-Based Access Control: Fine-grained resource permissions

### Common Vulnerability Prevention
- SQL Injection: Use parameterized queries and ORM
- XSS: Output encoding and Content Security Policy (CSP)
- CSRF: Anti-CSRF tokens and SameSite cookies
- Insecure Deserialization: Validate input types and structure

## Output Format

The skill produces:

1. **Security Architecture**: Complete security design for the application
2. **Authentication Flow**: Detailed login/logout and token management procedures
3. **Authorization Rules**: Role and permission definitions with implementation
4. **Validation Schemas**: Input validation and sanitization patterns
5. **Data Protection Strategy**: Encryption and data handling procedures
6. **Security Configuration**: Security headers, CORS, and communication settings
7. **Monitoring Setup**: Security logging and alerting mechanisms

## Example Implementation

### Input
```
Implement security for a task management application that handles user tasks and personal information
```

### Process
1. **Authentication**: JWT-based with refresh tokens, password strength requirements
2. **Authorization**: RBAC with user roles (admin, regular user), resource ownership checks
3. **Input Validation**: Schema validation for all API requests, XSS protection
4. **Data Protection**: Encrypt sensitive data, mask PII in logs
5. **Communication Security**: HTTPS enforcement, secure headers, CORS policy
6. **Monitoring**: Log security events, implement rate limiting, set up alerts

### Output
```json
{
  "authentication": {
    "method": "JWT",
    "tokenExpiry": "15m",
    "refreshExpiry": "7d",
    "passwordPolicy": {
      "minLength": 8,
      "requireSpecialChar": true,
      "requireNumber": true,
      "requireUppercase": true
    }
  },
  "authorization": {
    "model": "RBAC",
    "roles": ["admin", "user"],
    "permissions": {
      "user": ["read:own_tasks", "create:tasks", "update:own_tasks", "delete:own_tasks"],
      "admin": ["read:all_tasks", "create:tasks", "update:all_tasks", "delete:all_tasks", "manage:users"]
    }
  },
  "validation": {
    "inputSanitization": true,
    "schemaValidation": true,
    "rateLimiting": {
      "requests": 100,
      "window": "15m"
    }
  },
  "dataProtection": {
    "encryptionAtRest": true,
    "encryptionInTransit": true,
    "dataMasking": {
      "email": "j***@e***.com",
      "phone": "+1***5678"
    }
  },
  "securityHeaders": {
    "HSTS": "max-age=31536000; includeSubDomains",
    "X-Frame-Options": "DENY",
    "X-Content-Type-Options": "nosniff",
    "Content-Security-Policy": "default-src 'self'"
  }
}
```

## Best Practices

- Implement authentication and authorization at every layer
- Use established security libraries rather than custom implementations
- Regularly update dependencies to patch security vulnerabilities
- Conduct security code reviews and penetration testing
- Implement proper error handling without information leakage
- Use environment-specific security configurations
- Apply the principle of least privilege consistently
- Monitor and log security events for incident response

## Anti-Patterns to Avoid

- Storing passwords in plain text or with reversible encryption
- Using weak hashing algorithms (MD5, SHA-1)
- Trusting client-side validation as the only security measure
- Exposing sensitive information in error messages
- Hardcoding secrets in source code
- Using default credentials or weak passwords
- Allowing unlimited request rates without rate limiting
- Implementing custom cryptographic algorithms