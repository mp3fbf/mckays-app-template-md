# Hooks Directory

The `/hooks` directory contains custom React hooks used throughout the application. These hooks encapsulate reusable stateful logic that can be shared between components.

## Structure

```plaintext
/hooks
  ├── use-debounce.ts          # Debounce functionality
  ├── use-local-storage.ts     # Local storage interactions
  ├── use-media-query.ts       # Media query detection
  └── use-mount.ts             # Component mounting utilities
```

## Example Hooks

### Debounce Hook

```typescript
// hooks/use-debounce.ts
import { useEffect, useState } from "react"

export function useDebounce<T>(value: T, delay?: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedValue(value)
    }, delay || 500)

    return () => {
      clearTimeout(timer)
    }
  }, [value, delay])

  return debouncedValue
}
```

### Local Storage Hook

```typescript
// hooks/use-local-storage.ts
import { useState, useEffect } from "react"

export function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    if (typeof window === "undefined") return initialValue

    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : initialValue
    } catch (error) {
      return initialValue
    }
  })

  useEffect(() => {
    if (typeof window === "undefined") return

    try {
      window.localStorage.setItem(key, JSON.stringify(value))
    } catch (error) {
      console.error(error)
    }
  }, [key, value])

  return [value, setValue] as const
}
```

## Usage Examples

The hooks can be used in components like this:

```typescript
// Example component using hooks
import { useDebounce } from "@/hooks/use-debounce"
import { useLocalStorage } from "@/hooks/use-local-storage"

export function SearchComponent() {
  const [search, setSearch] = useState("")
  const debouncedSearch = useDebounce(search, 500)
  const [recentSearches, setRecentSearches] = useLocalStorage<string[]>("recent-searches", [])

  useEffect(() => {
    if (debouncedSearch) {
      setRecentSearches(prev => [...prev, debouncedSearch])
    }
  }, [debouncedSearch])

  return (
    // Component JSX
  )
}
```

## Best Practices

1. **Naming Conventions**
   - Prefix custom hooks with "use"
   - Use descriptive names that indicate functionality
   - Keep naming consistent across the application

2. **Type Safety**
   - Use TypeScript generics where appropriate
   - Define proper return types
   - Handle edge cases and errors

3. **Performance**
   - Optimize dependencies in useEffect
   - Memoize values when needed
   - Clean up side effects properly

4. **Testing**
   - Write tests for custom hooks
   - Test different states and scenarios
   - Mock browser APIs when needed

## Common Hook Categories

1. **State Management**
   - Form state
   - UI state
   - Data fetching state

2. **Browser APIs**
   - Local storage
   - Session storage
   - Media queries
   - Viewport dimensions

3. **Performance**
   - Debouncing
   - Throttling
   - Memoization

4. **Lifecycle**
   - Mounting/unmounting
   - Updates
   - Cleanup