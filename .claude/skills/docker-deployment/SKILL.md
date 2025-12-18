---
name: "docker-deployment"
description: "Implement production-grade Docker deployment strategies with multi-stage builds, security scanning, orchestration, and CI/CD integration for scalable applications"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade Docker Deployment Skill

## When to Use This Skill

Use this skill when implementing containerized deployments for:
- Applications requiring consistent environments across development, staging, and production
- Systems that need to scale horizontally with container orchestration
- Projects requiring isolated, reproducible deployments
- Applications that must meet security and compliance standards
- Services that need to integrate with cloud platforms and orchestration tools
- Teams that want to streamline deployment processes and reduce environment inconsistencies

## Core Principles

1. **Security First**: Implement secure container images with minimal attack surface
2. **Optimization**: Use multi-stage builds to minimize image size and attack surface
3. **Consistency**: Ensure identical behavior across all environments
4. **Efficiency**: Optimize build processes and resource utilization
5. **Reliability**: Implement health checks and proper container lifecycle management
6. **Scalability**: Design for horizontal scaling and load distribution
7. **Observability**: Include proper logging, monitoring, and debugging capabilities

## Process Steps

### 1. Multi-Stage Build Configuration
- Create optimized Dockerfile with separate build and runtime stages
- Use minimal base images (Alpine, Distroless) for production
- Remove build dependencies and unnecessary files from final image
- Implement build caching strategies to speed up builds
- Set up proper user permissions and security contexts

### 2. Container Security Implementation
- Run containers as non-root users
- Implement read-only filesystems where possible
- Scan images for vulnerabilities using tools like Trivy or Clair
- Sign images using Docker Content Trust or similar
- Implement security policies and runtime constraints

### 3. Environment Configuration
- Use environment variables for configuration management
- Implement secrets management for sensitive data
- Create environment-specific configuration files
- Set up proper resource limits and requests
- Configure health checks and liveness probes

### 4. Orchestration Setup
- Design Docker Compose files for local development
- Create Kubernetes manifests for production deployment
- Implement service discovery and load balancing
- Set up persistent storage and volume management
- Configure networking and service mesh if needed

### 5. CI/CD Integration
- Integrate Docker builds into CI/CD pipelines
- Implement automated security scanning
- Set up image registry management
- Configure deployment automation and rollback strategies
- Implement blue-green or canary deployment patterns

### 6. Monitoring and Observability
- Configure structured logging with centralized collection
- Implement metrics collection and monitoring
- Set up distributed tracing for microservices
- Create dashboard and alerting configurations
- Plan for log aggregation and analysis

## Docker Implementation Framework

### Multi-Stage Build Patterns
- Build stage: Install dependencies, compile code
- Test stage: Run tests and quality checks
- Production stage: Copy artifacts from build stage
- Final optimization: Remove unnecessary files and set permissions

### Security Best Practices
- Use official, minimal base images
- Implement non-root user execution
- Apply least privilege principle
- Regular security scanning and updates
- Image signing and verification

### Orchestration Patterns
- Service discovery and load balancing
- Health checks and auto-healing
- Persistent storage management
- Configuration management
- Secret management

## Output Format

The skill produces:

1. **Dockerfile**: Optimized multi-stage build configuration
2. **Docker Compose**: Local development and testing environment
3. **Kubernetes Manifests**: Production deployment configurations
4. **Security Configuration**: Security policies and scanning setup
5. **CI/CD Integration**: Pipeline configuration for automated builds
6. **Monitoring Setup**: Logging, metrics, and observability configuration
7. **Deployment Strategy**: Rollout and rollback procedures

## Example Implementation

### Input
```
Implement Docker deployment for a full-stack task management application with Next.js frontend and FastAPI backend
```

### Process
1. **Multi-Stage Build**: Separate build and production stages for both frontend and backend
2. **Security**: Non-root users, minimal base images, vulnerability scanning
3. **Configuration**: Environment variables and secrets management
4. **Orchestration**: Docker Compose for local, Kubernetes for production
5. **CI/CD**: Automated builds and deployments with security scanning
6. **Monitoring**: Structured logging and metrics collection

### Output
```dockerfile
# Multi-stage Dockerfile for Next.js frontend
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine AS production
WORKDIR /app

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

# Copy built application from builder stage
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder --chown=nextjs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nextjs:nodejs /app/package*.json ./

USER nextjs

EXPOSE 3000

CMD ["npm", "start"]

# Multi-stage Dockerfile for FastAPI backend
# Build stage
FROM python:3.10-slim AS backend-builder
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade -r requirements.txt
COPY . .

# Production stage
FROM python:3.10-slim AS backend-production
WORKDIR /app

# Create non-root user
RUN useradd --create-home --shell /bin/bash app
USER app

# Copy dependencies and application from builder
COPY --from=backend-builder --chown=app:app /app /app
RUN pip install --user --no-cache-dir --upgrade -r requirements.txt

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml for local development
version: '3.8'
services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://localhost:8000
    depends_on:
      - backend
    volumes:
      - ./frontend:/app
      - /app/node_modules

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/taskdb
      - JWT_SECRET=dev-secret-key
    depends_on:
      - db
    volumes:
      - ./backend:/app

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=taskdb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

```yaml
# Kubernetes deployment for production
# frontend-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: myorg/frontend:latest
        ports:
        - containerPort: 3000
        env:
        - name: NEXT_PUBLIC_API_URL
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: api_url
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

## Best Practices

- Use multi-stage builds to minimize image size and attack surface
- Run containers as non-root users with minimal permissions
- Implement proper health checks for container orchestration
- Use environment variables for configuration, not hardcoded values
- Scan images for vulnerabilities before deployment
- Implement proper resource limits and requests
- Use official base images and keep them updated
- Implement proper logging with structured formats

## Anti-Patterns to Avoid

- Using latest tag for base images without version pinning
- Running containers as root user
- Storing secrets in Docker images or environment variables
- Using large base images when minimal ones are available
- Not implementing health checks for container orchestration
- Hardcoding configuration values in Dockerfiles
- Not setting resource limits leading to resource exhaustion
- Pushing images without security scanning