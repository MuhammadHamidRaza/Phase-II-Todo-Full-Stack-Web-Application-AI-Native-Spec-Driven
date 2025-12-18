---
name: "auth-jwt-patterns"
description: "Implement secure JWT authentication patterns with proper token management, refresh strategies, and security best practices for stateless authentication"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade JWT Authentication Patterns Skill

## When to Use This Skill

Use this skill when implementing authentication for:
- Stateless applications requiring scalable authentication
- Microservices architectures with distributed authentication
- Single Page Applications (SPAs) and mobile applications
- APIs that need to authenticate and authorize requests
- Systems requiring token-based session management
- Applications that need to support multiple client types
- Services that must maintain security while scaling

## Core Principles

1. **Security First**: Implement secure token generation, storage, and validation
2. **Statelessness**: Maintain authentication without server-side session storage
3. **Token Management**: Proper handling of access and refresh tokens
4. **Performance**: Efficient token validation without database lookups
5. **Flexibility**: Support for multiple authentication providers and strategies
6. **Compliance**: Follow JWT standards and security best practices
7. **User Experience**: Smooth authentication flow with minimal interruptions

## Process Steps

### 1. JWT Configuration and Setup
- Choose appropriate signing algorithm (RS256 preferred over HS256)
- Generate secure signing keys and implement key rotation
- Define token payload structure with necessary claims
- Set appropriate expiration times for different token types
- Implement proper key storage and management

### 2. Token Generation and Signing
- Create JWT with appropriate claims (sub, exp, iat, etc.)
- Include user roles and permissions in token payload
- Implement proper token signing with secure algorithms
- Add necessary metadata for token validation
- Ensure token size remains reasonable for network efficiency

### 3. Token Storage and Management
- Implement secure storage for access and refresh tokens
- Use HttpOnly, Secure, SameSite cookies for web applications
- Store tokens in secure local storage for SPAs when appropriate
- Implement token refresh strategies to maintain sessions
- Handle token expiration and renewal gracefully

### 4. Token Validation and Verification
- Verify token signature using public keys (for RS256)
- Validate token expiration and other standard claims
- Implement token revocation mechanisms when needed
- Add custom validation logic for application-specific requirements
- Handle token validation errors appropriately

### 5. Refresh Token Management
- Implement secure refresh token storage and validation
- Create refresh token rotation strategies
- Implement refresh token revocation on logout
- Add refresh token expiration and renewal logic
- Secure refresh tokens with appropriate security measures

### 6. Security Implementation
- Implement CSRF protection for web applications
- Add rate limiting to prevent token brute-force attacks
- Implement token blacklisting for compromised tokens
- Add security headers for token transmission
- Monitor and log authentication attempts

## JWT Implementation Framework

### Token Structure
- Header: Algorithm and token type
- Payload: Claims including user identity, roles, permissions
- Signature: Verification of token integrity

### Claim Standards
- Standard claims: sub, exp, iat, iss, aud
- Custom claims: user roles, permissions, tenant ID
- Private claims: application-specific data

### Token Types
- Access tokens: Short-lived, for API access
- Refresh tokens: Long-lived, for token renewal
- ID tokens: For user identity information

## Output Format

The skill produces:

1. **JWT Configuration**: Complete token generation and validation setup
2. **Token Management**: Storage and refresh strategies
3. **Security Implementation**: Protection mechanisms and validation
4. **Integration Patterns**: Middleware and authentication flows
5. **Revocation Strategy**: Token invalidation and blacklisting
6. **Monitoring Setup**: Authentication logging and metrics
7. **Client Implementation**: Frontend integration patterns

## Example Implementation

### Input
```
Implement JWT authentication for a task management application with user roles and permissions
```

### Process
1. **JWT Setup**: RS256 algorithm with secure key management
2. **Token Structure**: User ID, roles, permissions, and metadata
3. **Storage Strategy**: HttpOnly cookies for web, local storage for mobile
4. **Validation**: Middleware for token verification and role checking
5. **Refresh**: Secure refresh token implementation with rotation
6. **Security**: CSRF protection and rate limiting

### Output
```javascript
// JWT Configuration
const jwtConfig = {
  algorithm: 'RS256',
  accessTokenExpiry: '15m',
  refreshTokenExpiry: '7d',
  issuer: 'task-management-app',
  audience: 'task-management-users'
};

// Token Generation
function generateTokens(user) {
  const accessToken = jwt.sign(
    {
      sub: user.id,
      email: user.email,
      role: user.role,
      permissions: user.permissions,
      exp: Math.floor(Date.now() / 1000) + (15 * 60), // 15 minutes
    },
    privateKey,
    {
      algorithm: 'RS256',
      issuer: jwtConfig.issuer,
      audience: jwtConfig.audience
    }
  );

  const refreshToken = jwt.sign(
    {
      sub: user.id,
      type: 'refresh',
      exp: Math.floor(Date.now() / 1000) + (7 * 24 * 60 * 60), // 7 days
    },
    refreshTokenSecret,
    {
      algorithm: 'HS512',
      issuer: jwtConfig.issuer
    }
  );

  return { accessToken, refreshToken };
}

// Token Validation Middleware
function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1]; // Bearer TOKEN

  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }

  jwt.verify(token, publicKey, {
    algorithms: ['RS256'],
    issuer: jwtConfig.issuer,
    audience: jwtConfig.audience
  }, (err, decoded) => {
    if (err) {
      if (err.name === 'TokenExpiredError') {
        return res.status(401).json({ error: 'Token expired' });
      }
      return res.status(403).json({ error: 'Invalid token' });
    }

    req.user = decoded;
    next();
  });
}

// Refresh Token Handler
async function refreshAccessToken(refreshToken) {
  try {
    const decoded = jwt.verify(refreshToken, refreshTokenSecret, {
      algorithms: ['HS512'],
      issuer: jwtConfig.issuer
    });

    // Verify refresh token hasn't been revoked
    const isRevoked = await isRefreshTokenRevoked(decoded.sub, refreshToken);
    if (isRevoked) {
      throw new Error('Refresh token revoked');
    }

    // Get user from database
    const user = await getUserById(decoded.sub);
    if (!user) {
      throw new Error('User not found');
    }

    // Generate new tokens
    const { accessToken, refreshToken: newRefreshToken } = generateTokens(user);

    // Store new refresh token and revoke old one
    await revokeRefreshToken(decoded.sub, refreshToken);
    await storeRefreshToken(user.id, newRefreshToken);

    return { accessToken, refreshToken: newRefreshToken };
  } catch (error) {
    throw new Error('Invalid refresh token');
  }
}

// Express.js Middleware Example
app.post('/auth/refresh', async (req, res) => {
  const { refreshToken } = req.body;

  if (!refreshToken) {
    return res.status(401).json({ error: 'Refresh token required' });
  }

  try {
    const tokens = await refreshAccessToken(refreshToken);
    res.json({
      accessToken: tokens.accessToken,
      refreshToken: tokens.refreshToken
    });
  } catch (error) {
    res.status(403).json({ error: 'Invalid refresh token' });
  }
});
```

## Best Practices

- Use RS256 algorithm instead of HS256 for better security
- Implement proper key rotation strategies
- Store refresh tokens securely in a database with appropriate indexing
- Use short-lived access tokens (15-30 minutes) with refresh tokens
- Implement token blacklisting for logout and compromised tokens
- Add CSRF protection when using JWT with web applications
- Validate all JWT claims including issuer, audience, and expiration
- Use appropriate HTTP status codes for authentication errors

## Anti-Patterns to Avoid

- Storing sensitive information in JWT payload without encryption
- Using HS256 algorithm with weak secrets
- Not implementing token expiration or using very long expiration times
- Storing refresh tokens in local storage where they can be accessed by XSS
- Not validating JWT audience and issuer claims
- Using JWT for session management without proper security measures
- Not implementing token revocation mechanisms
- Hardcoding JWT secrets in source code