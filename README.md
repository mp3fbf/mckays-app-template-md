# Mckay's App Template - Documentation

This is a markdown version of the [mckays-app-template](https://github.com/mckaywrigley/mckays-app-template) repository, generated for use as a knowledge base in AI projects.

## Directory Structure Documentation

- [Actions](/docs/actions.md) - Server actions for data fetching and mutations
- [App](/docs/app.md) - Next.js application code and routes
- [Components](/docs/components.md) - React components using shadcn/ui
- [Database](/docs/db.md) - Database schemas and queries using Drizzle ORM
- [Hooks](/docs/hooks.md) - React hooks and custom hooks
- [Library](/docs/lib.md) - Utility functions and helpers
- [Prompts](/docs/prompts.md) - AI prompt templates and configurations
- [Types](/docs/types.md) - TypeScript type definitions

## Tech Stack

- IDE: [Cursor](https://www.cursor.com/)
- AI Tools: [V0](https://v0.dev/), [Perplexity](https://www.perplexity.com/)
- Frontend: [Next.js](https://nextjs.org/docs), [Tailwind](https://tailwindcss.com/docs/guides/nextjs), [Shadcn](https://ui.shadcn.com/docs/installation), [Framer Motion](https://www.framer.com/motion/introduction/)
- Backend: [PostgreSQL](https://www.postgresql.org/about/), [Supabase](https://supabase.com/), [Drizzle](https://orm.drizzle.team/docs/get-started-postgresql), [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)
- Auth: [Clerk](https://clerk.com/)
- Payments: [Stripe](https://stripe.com/)
- Analytics: [PostHog](https://posthog.com/)

## Prerequisites

You will need accounts for the following services:

- Create a [Cursor](https://www.cursor.com/) account
- Create a [GitHub](https://github.com/) account
- Create a [Supabase](https://supabase.com/) account
- Create a [Clerk](https://clerk.com/) account
- Create a [Stripe](https://stripe.com/) account
- Create a [PostHog](https://posthog.com/) account
- Create a [Vercel](https://vercel.com/) account

## Environment Setup

Required environment variables:

```env
# DB
DATABASE_URL=

# Auth
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/login
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/signup

# Payments
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
NEXT_PUBLIC_STRIPE_PORTAL_LINK=
NEXT_PUBLIC_STRIPE_PAYMENT_LINK_YEARLY=
NEXT_PUBLIC_STRIPE_PAYMENT_LINK_MONTHLY=

# Analytics
NEXT_PUBLIC_POSTHOG_KEY=
NEXT_PUBLIC_POSTHOG_HOST=
```

## Getting Started

1. Clone the repository
2. Copy `.env.example` to `.env.local` and fill in the environment variables
3. Install dependencies with `npm install`
4. Start the development server with `npm run dev`

## Key Features

1. **Authentication**
   - User authentication via Clerk
   - Protected routes
   - User profiles

2. **Database**
   - PostgreSQL database hosted on Supabase
   - Type-safe queries with Drizzle ORM
   - Automatic migrations

3. **UI Components**
   - Shadcn UI components
   - Responsive design with Tailwind
   - Animations with Framer Motion

4. **Payments**
   - Stripe integration
   - Subscription management
   - Payment processing

5. **Analytics**
   - PostHog tracking
   - User behavior analysis
   - Conversion tracking

## Additional Resources

For more detailed information about specific aspects of the template, visit:

- [Next.js Documentation](https://nextjs.org/docs)
- [Tailwind Documentation](https://tailwindcss.com/docs)
- [Shadcn UI Documentation](https://ui.shadcn.com/docs)
- [Drizzle ORM Documentation](https://orm.drizzle.team/docs/overview)
- [Clerk Documentation](https://clerk.com/docs)
- [Stripe Documentation](https://stripe.com/docs)
- [PostHog Documentation](https://posthog.com/docs)