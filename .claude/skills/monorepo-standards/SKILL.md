---
name: "monorepo-standards"
description: "Implement production-grade monorepo standards with proper project structure, dependency management, build optimization, and team collaboration patterns"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade Monorepo Standards Skill

## When to Use This Skill

Use this skill when setting up monorepo architectures for:
- Organizations with multiple related projects that benefit from shared code
- Teams that want to simplify dependency management and versioning
- Projects requiring consistent coding standards and tooling across services
- Systems that need to coordinate changes across multiple services
- Organizations looking to improve developer productivity and code sharing
- Applications that benefit from atomic commits across multiple packages
- Teams that want to streamline CI/CD and deployment processes

## Core Principles

1. **Modularity**: Maintain clear boundaries between different packages and services
2. **Consistency**: Apply consistent standards, tooling, and practices across all packages
3. **Performance**: Optimize build times and dependency management for large codebases
4. **Scalability**: Design architecture that can accommodate growth in team and code size
5. **Maintainability**: Create clear project structure and documentation for onboarding
6. **Isolation**: Ensure packages can be developed and tested independently
7. **Collaboration**: Enable multiple teams to work efficiently in the same repository

## Process Steps

### 1. Project Structure Design
- Define clear directory structure for different packages and services
- Establish naming conventions for packages and modules
- Plan for different types of packages (apps, libraries, tools)
- Create shared configuration files and tooling
- Design workspace organization for build tools (pnpm, yarn, npm)

### 2. Dependency Management
- Implement workspace configuration for package managers
- Establish shared dependencies and version management
- Create private package publishing and consumption patterns
- Set up dependency update and security scanning processes
- Plan for monorepo-specific dependency challenges

### 3. Build and Development Setup
- Configure build tools for incremental builds and caching
- Set up development servers with hot reloading
- Implement cross-package linking and development workflows
- Create build optimization strategies for large monorepos
- Plan for parallel builds and resource management

### 4. Testing and Quality Assurance
- Set up testing frameworks that work across packages
- Implement cross-package integration testing
- Create quality gates and linting standards
- Establish code coverage requirements and reporting
- Plan for test parallelization and performance

### 5. CI/CD Integration
- Configure CI/CD pipelines for monorepo-specific needs
- Implement smart change detection and affected package analysis
- Set up automated publishing for packages
- Create deployment strategies for different services
- Plan for environment-specific configurations

### 6. Team Collaboration
- Establish branching and merging strategies
- Create contribution guidelines for different packages
- Set up code ownership and review processes
- Plan for team-specific access controls and workflows
- Document monorepo-specific development practices

## Monorepo Implementation Framework

### Structure Patterns
- Apps directory: Contains application packages
- Packages directory: Reusable libraries and components
- Tools directory: Development and build tools
- Config directory: Shared configuration files
- Scripts directory: Build and automation scripts

### Tooling Stack
- Package managers: pnpm, yarn workspaces, or npm workspaces
- Build tools: Turborepo, Nx, or Lerna for optimization
- Testing: Jest, Vitest, or other frameworks with cross-package support
- Linting: ESLint, Prettier with shared configurations
- Documentation: Storybook, Docusaurus, or similar tools

### Dependency Patterns
- Internal dependencies: Cross-package references within monorepo
- External dependencies: Managed at workspace or package level
- Peer dependencies: For library packages with shared requirements
- Dev dependencies: Shared across packages when appropriate

## Output Format

The skill produces:

1. **Project Structure**: Complete directory organization and naming conventions
2. **Dependency Management**: Workspace configuration and versioning strategy
3. **Build Configuration**: Optimized build tools and caching setup
4. **Testing Strategy**: Cross-package testing and quality assurance
5. **CI/CD Pipeline**: Monorepo-specific automation and deployment
6. **Team Workflow**: Collaboration and contribution guidelines
7. **Performance Optimization**: Build and development performance tuning

## Example Implementation

### Input
```
Implement monorepo standards for a task management application with Next.js frontend, FastAPI backend, shared UI components, and common utilities
```

### Process
1. **Structure**: Organize apps (frontend, backend) and packages (ui, utils, types)
2. **Dependencies**: Configure pnpm workspaces and shared dependencies
3. **Build**: Set up Turborepo for optimized builds and caching
4. **Testing**: Configure cross-package testing with shared utilities
5. **CI/CD**: Implement affected package detection and selective builds
6. **Standards**: Create consistent linting, formatting, and documentation

### Output
```
task-management-monorepo/
├── apps/
│   ├── frontend/                 # Next.js frontend application
│   │   ├── package.json
│   │   ├── src/
│   │   └── turbo.json
│   └── backend/                  # FastAPI backend application
│       ├── pyproject.toml
│       ├── src/
│       └── docker/
├── packages/
│   ├── ui/                       # Shared UI components
│   │   ├── package.json
│   │   ├── src/
│   │   └── stories/
│   ├── utils/                    # Shared utility functions
│   │   ├── package.json
│   │   └── src/
│   ├── types/                    # Shared TypeScript types
│   │   ├── package.json
│   │   └── src/
│   └── config/                   # Shared configurations
│       ├── package.json
│       └── eslint-config-custom/
├── tools/
│   ├── scripts/                  # Build and automation scripts
│   └── dev-utils/               # Development utilities
├── docs/                        # Documentation
├── .github/
│   └── workflows/               # CI/CD workflows
├── pnpm-workspace.yaml          # pnpm workspace configuration
├── turbo.json                   # Turborepo configuration
├── package.json                 # Root package configuration
├── nx.json                      # Nx configuration (if using Nx)
└── README.md

# pnpm-workspace.yaml
packages:
  - "apps/*"
  - "packages/*"
  - "tools/*"

# turbo.json (root)
{
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "dist/**", "build/**"]
    },
    "test": {
      "dependsOn": [],
      "outputs": []
    },
    "lint": {
      "outputs": []
    },
    "dev": {
      "cache": false
    }
  }
}

# packages/ui/package.json
{
  "name": "@task-manager/ui",
  "version": "1.0.0",
  "dependencies": {
    "react": "^18.0.0",
    "tailwindcss": "^3.0.0",
    "@task-manager/types": "workspace:*",
    "@task-manager/utils": "workspace:*"
  },
  "devDependencies": {
    "@task-manager/config": "workspace:*"
  }
}

# apps/frontend/package.json
{
  "name": "@task-manager/frontend",
  "version": "1.0.0",
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0",
    "@task-manager/ui": "workspace:*",
    "@task-manager/types": "workspace:*",
    "@task-manager/utils": "workspace:*"
  },
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "test": "turbo run test",
    "lint": "turbo run lint"
  }
}

# CI/CD GitHub Actions
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0  # Required for change detection

      - uses: pnpm/action-setup@v2
        with:
          version: 8

      - uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'pnpm'

      - name: Install dependencies
        run: pnpm install

      - name: Build affected packages
        run: pnpm turbo run build --filter=--since=${{ github.event.before }}

      - name: Run tests for affected packages
        run: pnpm turbo run test --filter=--since=${{ github.event.before }}
```

## Best Practices

- Use clear and consistent naming conventions for packages
- Implement smart build systems that only build affected packages
- Maintain shared configurations for consistency across packages
- Use workspace protocols for internal dependencies (workspace:*)
- Set up proper cross-package testing and integration validation
- Document package boundaries and responsibilities clearly
- Implement proper access controls and code ownership
- Regularly audit and clean up unused dependencies

## Anti-Patterns to Avoid

- Creating circular dependencies between packages
- Not implementing proper change detection in CI/CD
- Having inconsistent tooling and configurations across packages
- Not setting up proper build caching and optimization
- Creating packages that are too granular or too large
- Not planning for team scaling and collaboration challenges
- Ignoring monorepo-specific performance considerations
- Not establishing clear package ownership and responsibilities