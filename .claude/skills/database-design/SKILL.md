---
name: "database-design"
description: "Design production-grade database schemas with proper normalization, indexing, relationships, and performance optimization for scalable applications"
version: "1.0.0"
author: "Claude Code"
---

# Production-Grade Database Design Skill

## When to Use This Skill

Use this skill when designing databases for:
- Applications requiring scalable and maintainable data storage
- Systems that need to handle high transaction volumes
- Projects requiring complex data relationships and queries
- Applications that must maintain data integrity and consistency
- Systems that need to support reporting and analytics
- Services requiring high availability and performance

## Core Principles

1. **Normalization**: Apply appropriate normalization levels to eliminate redundancy
2. **Performance**: Optimize for query performance with proper indexing and structure
3. **Integrity**: Maintain data integrity through constraints and relationships
4. **Scalability**: Design schemas that can scale with application growth
5. **Maintainability**: Create clear, well-documented database structures
6. **Security**: Implement proper access controls and data protection
7. **Consistency**: Ensure consistent naming and structural patterns

## Process Steps

### 1. Requirements Analysis
- Identify core entities and their relationships
- Determine data access patterns and query requirements
- Analyze expected data volume and growth patterns
- Identify performance requirements and constraints
- Document business rules and validation requirements

### 2. Conceptual Design
- Create entity-relationship diagrams (ERDs)
- Define primary entities and their attributes
- Establish relationships between entities (1:1, 1:many, many:many)
- Identify business rules and constraints
- Plan for future requirements and extensibility

### 3. Logical Design
- Convert entities to tables with proper data types
- Define primary keys, foreign keys, and constraints
- Apply normalization rules (1NF, 2NF, 3NF)
- Plan indexing strategy for performance
- Design views and stored procedures where appropriate

### 4. Physical Design
- Choose appropriate database engine and configuration
- Implement partitioning strategies for large tables
- Configure backup and recovery procedures
- Set up monitoring and performance tracking
- Plan for high availability and disaster recovery

### 5. Optimization and Tuning
- Analyze query execution plans
- Implement appropriate indexes for common queries
- Optimize database configuration settings
- Plan for caching strategies
- Monitor and tune performance regularly

### 6. Security and Access Control
- Implement user roles and permissions
- Design secure access patterns
- Plan for data encryption and protection
- Implement audit logging for sensitive operations
- Secure database connections and credentials

## Database Design Framework

### Normalization Guidelines
- First Normal Form (1NF): Eliminate repeating groups
- Second Normal Form (2NF): Remove partial dependencies
- Third Normal Form (3NF): Remove transitive dependencies
- Consider denormalization for performance when appropriate

### Indexing Strategy
- Primary indexes on primary keys
- Foreign key indexes for relationship queries
- Composite indexes for multi-column searches
- Unique indexes for constraints
- Consider covering indexes for frequently accessed columns

### Relationship Patterns
- One-to-One: Foreign key in one table
- One-to-Many: Foreign key in the "many" table
- Many-to-Many: Junction table with foreign keys
- Self-referencing: For hierarchical data structures

## Output Format

The skill produces:

1. **Database Schema**: Complete table structure with relationships
2. **Indexing Strategy**: Performance optimization plan
3. **Constraints and Validation**: Data integrity rules
4. **Security Configuration**: Access controls and permissions
5. **Performance Tuning**: Optimization recommendations
6. **Backup and Recovery**: Data protection procedures
7. **Monitoring Setup**: Performance and health tracking

## Example Implementation

### Input
```
Design a database schema for a task management application with users, tasks, categories, and teams
```

### Process
1. **Entities Identified**: users, tasks, categories, teams, team_members
2. **Relationships**: Users have many tasks, tasks belong to categories, users belong to teams
3. **Normalization**: Apply 3NF to eliminate redundancy
4. **Indexing**: Primary keys, foreign keys, frequently queried columns
5. **Security**: Row-level security for user data isolation
6. **Performance**: Optimize for common task queries and filtering

### Output
```sql
-- Users table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Categories table
CREATE TABLE categories (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Teams table
CREATE TABLE teams (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    created_by UUID NOT NULL REFERENCES users(id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Team members junction table
CREATE TABLE team_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    team_id UUID NOT NULL REFERENCES teams(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role VARCHAR(50) DEFAULT 'member',
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(team_id, user_id)
);

-- Tasks table
CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    status VARCHAR(20) DEFAULT 'pending',
    priority VARCHAR(20) DEFAULT 'medium',
    due_date TIMESTAMP,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    assigned_to UUID REFERENCES users(id),
    category_id UUID REFERENCES categories(id),
    team_id UUID REFERENCES teams(id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Indexes for performance
CREATE INDEX idx_tasks_user_id ON tasks(user_id);
CREATE INDEX idx_tasks_assigned_to ON tasks(assigned_to);
CREATE INDEX idx_tasks_status ON tasks(status);
CREATE INDEX idx_tasks_due_date ON tasks(due_date);
CREATE INDEX idx_tasks_team_id ON tasks(team_id);
CREATE INDEX idx_users_email ON users(email);

-- Constraints
ALTER TABLE tasks ADD CONSTRAINT chk_task_status CHECK (status IN ('pending', 'in_progress', 'completed', 'cancelled'));
ALTER TABLE tasks ADD CONSTRAINT chk_task_priority CHECK (priority IN ('low', 'medium', 'high', 'urgent'));
```

## Best Practices

- Use meaningful, consistent naming conventions for tables and columns
- Choose appropriate data types to optimize storage and performance
- Implement proper foreign key constraints to maintain referential integrity
- Design indexes based on actual query patterns, not assumptions
- Plan for data growth and implement partitioning when necessary
- Document database schema with clear comments and ERDs
- Regularly review and optimize database performance
- Implement proper backup and recovery procedures

## Anti-Patterns to Avoid

- Using generic names like "id", "data", "info" for columns
- Creating tables with too many columns (wide tables)
- Not using foreign key constraints to maintain data integrity
- Creating indexes without understanding query patterns
- Storing large binary data directly in database tables
- Using multiple databases when a single database would suffice
- Not planning for data archival and retention policies
- Hardcoding database-specific features that reduce portability