# Library (lib) Directory

The `/lib` directory contains utility functions, configuration, and shared code used throughout the application. This directory houses foundational code that doesn't fit into components or hooks.

## Structure

```plaintext
/lib
  ├── utils.ts           # General utility functions
  ├── db.ts             # Database connection and utilities
  ├── auth.ts           # Authentication utilities
  └── api/              # API utilities and types
```

## Core Features

### 1. Utility Functions (utils.ts)

The utils.ts file contains general utility functions, with a key example being the `cn` function for managing Tailwind CSS classes:

```typescript
import { type ClassValue, clsx } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

This function combines [clsx](https://github.com/lukeed/clsx) and [tailwind-merge](https://github.com/dcastil/tailwind-merge) to:
- Merge CSS class names
- Handle conditional classes
- Resolve Tailwind CSS conflicts

### 2. Database Configuration (db.ts)

Database setup and configuration using Drizzle ORM:

```typescript
import { drizzle } from "drizzle-orm/postgres-js"
import postgres from "postgres"

const connectionString = process.env.DATABASE_URL!
const client = postgres(connectionString)
export const db = drizzle(client)
```

### 3. Authentication Utilities (auth.ts)

Authentication helpers using Clerk:

```typescript
import { auth } from "@clerk/nextjs"
import { redirect } from "next/navigation"

export function requireAuth() {
  const { userId } = auth()
  
  if (!userId) {
    redirect("/login")
  }
  
  return userId
}
```

## Common Utilities

1. **String Manipulation**
   - Text formatting
   - String validation
   - URL manipulation

2. **Date Handling**
   - Date formatting
   - Timezone conversions
   - Date calculations

3. **Number Formatting**
   - Currency formatting
   - Number validation
   - Mathematical utilities

4. **Data Transformation**
   - Object manipulation
   - Array transformations
   - Type conversions

## API Utilities

The api/ directory contains utilities for working with external APIs:

```typescript
// lib/api/fetch.ts
export async function fetchWithAuth(url: string, options: RequestInit = {}) {
  const response = await fetch(url, {
    ...options,
    headers: {
      ...options.headers,
      "Authorization": `Bearer ${process.env.API_KEY}`
    }
  })
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`)
  }
  
  return response.json()
}
```

## Constants and Types

Common constants and TypeScript types used across the application:

```typescript
// lib/constants.ts
export const SITE_URL = process.env.NEXT_PUBLIC_SITE_URL || "http://localhost:3000"
export const MAX_FILE_SIZE = 5 * 1024 * 1024 // 5MB

// lib/types.ts
export interface User {
  id: string
  email: string
  name?: string
  createdAt: Date
}
```

## Best Practices

1. **Organization**
   - Keep utility functions focused and pure
   - Group related utilities in subdirectories
   - Use meaningful file names

2. **Type Safety**
   - Use TypeScript for all utilities
   - Define proper types and interfaces
   - Document complex type definitions

3. **Error Handling**
   - Implement proper error handling
   - Use custom error classes
   - Provide meaningful error messages

4. **Testing**
   - Write unit tests for utilities
   - Test edge cases
   - Maintain high test coverage

## Usage Guidelines

1. **When to Use /lib**
   - Shared utility functions
   - Configuration code
   - Type definitions
   - Constants

2. **When Not to Use /lib**
   - Component-specific code
   - Page-specific logic
   - State management
   - UI-related code