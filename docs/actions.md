# Actions Directory

The `/actions` directory contains server actions used for data fetching and mutations in the application. This follows the [Next.js Server Actions](https://nextjs.org/docs/app/api-reference/functions/server-actions) pattern.

Server Actions are asynchronous functions that are executed on the server. They are used to perform tasks like:
- Reading from and writing to the database
- Making API calls to external services
- Processing form submissions
- Handling file uploads
- And other server-side operations

## Usage

Server actions can be used directly in Server Components or passed to Client Components as props.

### Example Structure

```plaintext
/actions
  ├── auth/            # Authentication related actions
  ├── stripe/          # Stripe payment related actions  
  ├── user/            # User management actions
  └── utils/           # Utility functions for actions
```

### Example Usage

```typescript
// Example server action in /actions/user/update-profile.ts
'use server'

import { db } from '@/lib/db'
import { users } from '@/db/schema'

export async function updateUserProfile(userId: string, data: any) {
  try {
    await db
      .update(users)
      .set(data)
      .where(eq(users.id, userId))

    return { success: true }
  } catch (error) {
    return { error: 'Failed to update profile' }
  }
}
```

This action can then be imported and used in components:

```typescript
// Example usage in a component
import { updateUserProfile } from '@/actions/user/update-profile'

// In a Server Component
const response = await updateUserProfile(userId, data)

// Or in a Client Component
<form action={updateUserProfile}>
  {/* form fields */}
</form>
```