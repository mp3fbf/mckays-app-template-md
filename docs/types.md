# Types Directory

The `/types` directory contains TypeScript type definitions, interfaces, and type utilities used throughout the application. This ensures type safety and provides better development experience through code completion and error detection.

## Structure

```plaintext
/types
  ├── index.ts          # Main type exports
  ├── api/              # API-related types
  ├── db/               # Database types
  └── components/       # Component prop types
```

## Core Types

### Common Types (index.ts)

```typescript
// types/index.ts

// User related types
export interface User {
  id: string
  email: string
  name?: string
  role: "user" | "admin"
  createdAt: Date
  updatedAt: Date
}

// Common utility types
export type Nullable<T> = T | null
export type Optional<T> = T | undefined
export type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P]
}
```

### API Types

```typescript
// types/api/index.ts
export interface APIResponse<T = any> {
  success: boolean
  data?: T
  error?: {
    code: string
    message: string
  }
}

export interface PaginatedResponse<T> extends APIResponse {
  data: {
    items: T[]
    total: number
    page: number
    pageSize: number
    totalPages: number
  }
}
```

### Database Types

```typescript
// types/db/index.ts
import { InferModel } from 'drizzle-orm'
import * as schema from '@/db/schema'

export type User = InferModel<typeof schema.users>
export type Profile = InferModel<typeof schema.profiles>
export type NewUser = InferModel<typeof schema.users, "insert">
```

## Common Type Patterns

### 1. Component Props

```typescript
// types/components/button.ts
export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: "default" | "primary" | "secondary" | "ghost"
  size?: "sm" | "md" | "lg"
  isLoading?: boolean
  leftIcon?: React.ReactNode
  rightIcon?: React.ReactNode
}

// Usage in component
import type { ButtonProps } from "@/types/components/button"

export function Button({ variant = "default", size = "md", ...props }: ButtonProps) {
  // Component implementation
}
```

### 2. Form Types

```typescript
// types/forms/index.ts
export interface FormField<T = any> {
  name: string
  label: string
  type: "text" | "number" | "email" | "password"
  value: T
  validation?: {
    required?: boolean
    min?: number
    max?: number
    pattern?: RegExp
  }
}

export interface FormConfig {
  fields: FormField[]
  onSubmit: (data: Record<string, any>) => Promise<void>
  onError?: (error: Error) => void
}
```

## Type Utilities

### 1. Type Guards

```typescript
// types/guards.ts
export function isUser(value: any): value is User {
  return (
    typeof value === "object" &&
    typeof value.id === "string" &&
    typeof value.email === "string"
  )
}

export function isAPIError(value: any): value is APIError {
  return (
    typeof value === "object" &&
    typeof value.code === "string" &&
    typeof value.message === "string"
  )
}
```

### 2. Type Transformations

```typescript
// types/transformers.ts
export type ReadOnly<T> = {
  readonly [P in keyof T]: T[P]
}

export type Mutable<T> = {
  -readonly [P in keyof T]: T[P]
}

export type PickPartial<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>
```

## Best Practices

1. **Organization**
   - Group related types together
   - Use meaningful file names
   - Keep type definitions focused

2. **Documentation**
   - Document complex types
   - Include examples
   - Explain type parameters

3. **Naming Conventions**
   - Use PascalCase for interfaces and types
   - Use descriptive names
   - Add Type suffix for type aliases when appropriate

4. **Type Safety**
   - Avoid using `any`
   - Use strict null checks
   - Leverage union types and intersections

## Common Use Cases

1. **API Contracts**
   ```typescript
   interface APIEndpoints {
     "/users": {
       GET: {
         params: { id: string }
         response: User
       }
       POST: {
         body: NewUser
         response: User
       }
     }
   }
   ```

2. **State Management**
   ```typescript
   interface State {
     user: Nullable<User>
     isLoading: boolean
     error: Optional<Error>
   }

   type Action =
     | { type: "SET_USER"; payload: User }
     | { type: "CLEAR_USER" }
     | { type: "SET_ERROR"; payload: Error }
   ```

3. **Component Props**
   ```typescript
   interface Props {
     data: readonly Item[]
     onSelect: (item: Item) => void
     renderItem?: (item: Item) => React.ReactNode
   }
   ```