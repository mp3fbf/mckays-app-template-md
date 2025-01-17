# Database (DB) Directory

The `/db` directory contains the database schema, migrations, and queries using [Drizzle ORM](https://orm.drizzle.team/) with PostgreSQL (via Supabase).

## Structure

```plaintext
/db
  ├── schema.ts           # Database schema definitions
  ├── migrations/         # Generated migration files
  └── types/             # Generated TypeScript types
```

## Features

1. **Schema Definition**
   - Uses Drizzle's schema builder
   - Type-safe table and column definitions
   - Relations and constraints

2. **Migrations**
   - Automatic migration generation
   - Version control for database changes
   - Safe schema updates

3. **Type Safety**
   - Generated TypeScript types
   - Type-safe queries
   - Automatic type inference

## Example Schema

A typical schema definition might look like this:

```typescript
// db/schema.ts
import { pgTable, serial, text, timestamp, boolean } from "drizzle-orm/pg-core"

export const users = pgTable("users", {
  id: serial("id").primaryKey(),
  userId: text("user_id").notNull().unique(),
  email: text("email").notNull().unique(),
  name: text("name"),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at").defaultNow().notNull()
})

export const profiles = pgTable("profiles", {
  id: serial("id").primaryKey(),
  userId: text("user_id")
    .references(() => users.userId)
    .notNull(),
  bio: text("bio"),
  isComplete: boolean("is_complete").default(false).notNull(),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at").defaultNow().notNull()
})
```

## Configuration

The database configuration is managed through Drizzle's config file:

```typescript
// drizzle.config.ts
import type { Config } from "drizzle-kit"

export default {
  schema: "./db/schema.ts",
  out: "./db/migrations",
  driver: "pg",
  dbCredentials: {
    connectionString: process.env.DATABASE_URL!
  }
} satisfies Config
```

## Usage in Actions

The schema is used in server actions to perform database operations:

```typescript
// Example usage in an action
import { db } from "@/lib/db"
import { users, profiles } from "@/db/schema"
import { eq } from "drizzle-orm"

export async function getUserProfile(userId: string) {
  const result = await db.query.users.findFirst({
    where: eq(users.userId, userId),
    with: {
      profile: true
    }
  })
  
  return result
}
```

## Database Setup

1. **Environment Variables**
   ```env
   DATABASE_URL=your_postgres_connection_string
   ```

2. **Initial Setup**
   ```bash
   # Generate migrations
   npm run db:generate

   # Apply migrations
   npm run db:push
   ```

3. **Supabase Integration**
   - Uses Supabase as the PostgreSQL host
   - Handles database hosting and scaling
   - Provides additional features like realtime subscriptions

## Best Practices

1. **Schema Changes**
   - Always create migrations for schema changes
   - Test migrations in development first
   - Back up data before applying migrations
   - Use meaningful names for migrations

2. **Type Safety**
   - Use generated types for database operations
   - Validate input data before queries
   - Handle errors appropriately

3. **Performance**
   - Use indexes for frequently queried columns
   - Optimize large queries
   - Monitor query performance