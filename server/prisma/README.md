# Prisma Setup and Commands

## Overview
This document provides instructions for working with Prisma ORM in the Team Task Manager project.

## Database Models

### User
- **id**: Unique identifier (CUID)
- **name**: User's full name
- **email**: Unique email address
- **password**: Hashed password
- **role**: ADMIN or MEMBER (default: MEMBER)
- **createdAt**: Timestamp of creation
- **updatedAt**: Timestamp of last update

### Project
- **id**: Unique identifier (CUID)
- **title**: Project title
- **description**: Optional project description
- **createdById**: User who created the project (Foreign Key)
- **createdAt**: Timestamp of creation
- **updatedAt**: Timestamp of last update

### ProjectMember
- **id**: Unique identifier (CUID)
- **userId**: User ID (Foreign Key)
- **projectId**: Project ID (Foreign Key)
- **createdAt**: Timestamp of creation

### Task
- **id**: Unique identifier (CUID)
- **title**: Task title
- **description**: Optional task description
- **status**: TODO, IN_PROGRESS, or DONE (default: TODO)
- **priority**: LOW, MEDIUM, or HIGH (default: MEDIUM)
- **dueDate**: Optional due date
- **projectId**: Project ID (Foreign Key)
- **createdById**: User who created the task (Foreign Key)
- **assignedToId**: User assigned to the task (Foreign Key, optional)
- **createdAt**: Timestamp of creation
- **updatedAt**: Timestamp of last update

## Key Features

### Relationships
- **User** → **Project**: One-to-Many (Creator)
- **User** → **ProjectMember**: One-to-Many
- **Project** → **ProjectMember**: One-to-Many
- **Project** → **Task**: One-to-Many (Cascade delete)
- **User** → **Task**: One-to-Many (Creator & Assignee)

### Cascade Deletes
- When a **User** is deleted, all created **Projects** are deleted
- When a **Project** is deleted, all **ProjectMembers** and **Tasks** are deleted
- When a **User** is deleted, all created **Tasks** are deleted

### Set Null Deletes
- When a **User** is deleted, **Tasks** assigned to them have `assignedToId` set to null

### Indexes
- Added indexes on frequently queried fields for performance optimization:
  - `User.email`
  - `Project.createdById`
  - `ProjectMember.projectId`
  - `Task.projectId`, `Task.createdById`, `Task.assignedToId`, `Task.status`, `Task.priority`

## Prisma Commands

### 1. **Initialize Prisma** (Already done)
```bash
npm install @prisma/client prisma
npx prisma init
```

### 2. **Create Database and Schema**
First migration to create all tables:
```bash
npm run prisma:migrate
# or
npx prisma migrate dev --name init
```

### 3. **Create New Migration**
After schema changes:
```bash
npx prisma migrate dev --name <migration_name>
# Example:
npx prisma migrate dev --name add_user_role
```

### 4. **Apply Migrations**
In production:
```bash
npx prisma migrate deploy
```

### 5. **Generate Prisma Client**
After schema changes:
```bash
npm run prisma:generate
# or
npx prisma generate
```

### 6. **Prisma Studio**
Open interactive database viewer:
```bash
npm run prisma:studio
# or
npx prisma studio
```

### 7. **Reset Database** (Development only)
⚠️ **Warning**: This deletes all data
```bash
npx prisma migrate reset
```

### 8. **View Migrations**
```bash
npx prisma migrate status
```

### 9. **Validate Schema**
```bash
npx prisma validate
```

### 10. **Format Schema**
```bash
npx prisma format
```

## Setup Instructions

1. **Copy environment variables**:
   ```bash
   cp .env.example .env
   ```

2. **Update DATABASE_URL** in `.env`:
   ```
   DATABASE_URL="postgresql://user:password@localhost:5432/team_task_manager?schema=public"
   ```

3. **Run initial migration**:
   ```bash
   npm run prisma:migrate
   ```

4. **Generate Prisma Client**:
   ```bash
   npm run prisma:generate
   ```

5. **Start the server**:
   ```bash
   npm run dev
   ```

## Common Workflows

### Creating Tables
1. Update `server/prisma/schema.prisma`
2. Run: `npx prisma migrate dev --name <description>`
3. Prisma automatically generates Prisma Client types

### Modifying Tables
1. Update model in `schema.prisma`
2. Run: `npx prisma migrate dev --name <description>`
3. Review generated migration file
4. Test with Prisma Studio: `npm run prisma:studio`

### Rolling Back Migrations
```bash
# Remove last migration (development only)
npx prisma migrate resolve --rolled-back <migration_name>
```

## Prisma Client Usage in Code

```javascript
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

// Create
const user = await prisma.user.create({
  data: {
    name: "John Doe",
    email: "john@example.com",
    password: "hashed_password",
    role: "MEMBER"
  }
});

// Read
const users = await prisma.user.findMany();
const user = await prisma.user.findUnique({
  where: { email: "john@example.com" }
});

// Update
const updated = await prisma.user.update({
  where: { id: "user_id" },
  data: { name: "Jane Doe" }
});

// Delete
await prisma.user.delete({
  where: { id: "user_id" }
});

// Query with relations
const project = await prisma.project.findUnique({
  where: { id: "project_id" },
  include: {
    createdBy: true,
    members: { include: { user: true } },
    tasks: true
  }
});
```

## Troubleshooting

### Migration conflicts
```bash
npx prisma migrate resolve --rolled-back <migration_name>
npx prisma migrate dev
```

### Prisma Client out of sync
```bash
npx prisma generate
```

### Database connection issues
- Verify `DATABASE_URL` in `.env`
- Ensure PostgreSQL is running
- Check network/firewall settings

### Reset everything
```bash
npx prisma migrate reset
npm run prisma:generate
```

## References
- [Prisma Documentation](https://www.prisma.io/docs)
- [Prisma Schema Reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference)
- [Data Model](https://www.prisma.io/docs/concepts/components/prisma-schema/data-model)
