# Data Model: Todo Full-Stack Web Application

## Task Entity

**Fields**:
- `id` (Integer, Primary Key, Auto-increment) - Unique identifier for each task
- `user_id` (String, Foreign Key) - References the user who owns the task
- `title` (String, Not Null) - Title of the task (max 200 characters)
- `description` (Text, Nullable) - Optional detailed description of the task
- `priority` (String) - Priority level (values: 'low', 'medium', 'high', 'critical'; default: 'medium')
- `due_date` (DateTime, Nullable) - Optional due date for the task
- `completed` (Boolean) - Completion status (default: false)
- `tags` (JSON/Text Array) - Array of tags associated with the task
- `created_at` (DateTime) - Timestamp when the task was created
- `updated_at` (DateTime) - Timestamp when the task was last updated

**Relationships**:
- One-to-Many: User has many Tasks (via user_id foreign key)
- User entity managed by Better Auth

**Validation Rules**:
- Title must be 1-200 characters
- Description must be less than 1000 characters if provided
- Priority must be one of the allowed values
- Due date must be a valid future date if provided
- Tags array must not exceed 10 tags per task
- User_id must reference an existing user

**Indexes**:
- Index on user_id (for filtering by user)
- Index on completed (for status filtering)
- Index on priority (for priority-based queries)
- Index on due_date (for date-based queries)

## Database Schema

```sql
CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    user_id VARCHAR(255) NOT NULL,
    title VARCHAR(200) NOT NULL,
    description TEXT,
    priority VARCHAR(20) DEFAULT 'medium',
    due_date TIMESTAMP WITH TIME ZONE,
    completed BOOLEAN DEFAULT FALSE,
    tags TEXT[],
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Indexes
CREATE INDEX idx_tasks_user_id ON tasks(user_id);
CREATE INDEX idx_tasks_completed ON tasks(completed);
CREATE INDEX idx_tasks_priority ON tasks(priority);
CREATE INDEX idx_tasks_due_date ON tasks(due_date);
```

## State Transitions

**Task Status Transitions**:
- `incomplete` → `completed` (when user marks task as complete)
- `completed` → `incomplete` (when user unmarks task as complete)

**Lifecycle Events**:
- Task creation: Set created_at, updated_at to current timestamp
- Task update: Update updated_at to current timestamp
- Task completion: Update completed status and updated_at
- Task deletion: Remove record (with cascade if user is deleted)