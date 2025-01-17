# Mckay's App Template - Documentation

Este é o guia completo e detalhado do [mckays-app-template](https://github.com/mckaywrigley/mckays-app-template), um template full-stack para desenvolvimento web.

## Índice

- [Tech Stack e Pré-requisitos](#tech-stack-e-pré-requisitos)
- [Estrutura de Diretórios](#estrutura-de-diretórios)
- [Diretório Actions](#diretório-actions)
- [Diretório App](#diretório-app)
- [Diretório Components](#diretório-components)
- [Diretório Database](#diretório-database)
- [Diretório Hooks](#diretório-hooks)
- [Diretório Library](#diretório-library)
- [Diretório Prompts](#diretório-prompts)
- [Diretório Types](#diretório-types)

## Tech Stack e Pré-requisitos

### Tech Stack

- IDE: [Cursor](https://www.cursor.com/)
- AI Tools: [V0](https://v0.dev/), [Perplexity](https://www.perplexity.com/)
- Frontend: [Next.js](https://nextjs.org/docs), [Tailwind](https://tailwindcss.com/docs/guides/nextjs), [Shadcn](https://ui.shadcn.com/docs/installation), [Framer Motion](https://www.framer.com/motion/introduction/)
- Backend: [PostgreSQL](https://www.postgresql.org/about/), [Supabase](https://supabase.com/), [Drizzle](https://orm.drizzle.team/docs/get-started-postgresql), [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)
- Auth: [Clerk](https://clerk.com/)
- Payments: [Stripe](https://stripe.com/)
- Analytics: [PostHog](https://posthog.com/)

### Pré-requisitos

Você precisará criar contas nos seguintes serviços:

- [Cursor](https://www.cursor.com/)
- [GitHub](https://github.com/)
- [Supabase](https://supabase.com/)
- [Clerk](https://clerk.com/)
- [Stripe](https://stripe.com/)
- [PostHog](https://posthog.com/)
- [Vercel](https://vercel.com/)

### Variáveis de Ambiente

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

## Estrutura de Diretórios

```plaintext
/
├── actions/           # Server actions para data fetching e mutations
├── app/              # Código da aplicação Next.js e rotas
├── components/       # Componentes React usando shadcn/ui
├── db/              # Schemas e queries do banco de dados usando Drizzle
├── hooks/           # Hooks React e hooks customizados
├── lib/             # Funções utilitárias e helpers
├── prompts/         # Templates e configurações de prompts AI
└── types/           # Definições de tipos TypeScript
```

## Diretório Actions

O diretório `/actions` contém server actions para data fetching e mutations seguindo o padrão Next.js Server Actions.

### Estrutura

```plaintext
/actions
  ├── auth/            # Ações relacionadas à autenticação
  ├── stripe/          # Ações relacionadas a pagamentos Stripe
  ├── user/            # Ações de gerenciamento de usuários
  └── utils/           # Funções utilitárias para actions
```

### Exemplo de Server Action

```typescript
// actions/user/update-profile.ts
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

## Diretório App

O diretório `/app` contém o código principal da aplicação usando Next.js App Router.

### Estrutura

```plaintext
/app
  ├── layout.tsx           # Layout raiz
  ├── page.tsx            # Página inicial
  ├── globals.css         # Estilos globais
  ├── error.tsx           # Componente de erro
  ├── loading.tsx         # Estado de loading
  ├── not-found.tsx      # Página 404
  ├── (auth)/            # Rotas de autenticação
  │   ├── login/         # Página de login
  │   └── signup/        # Página de cadastro
  └── api/               # Rotas de API
```

### Root Layout (layout.tsx)

```typescript
// app/layout.tsx
import { ClerkProvider } from "@clerk/nextjs"
import { Providers } from "@/components/utilities/providers"

export const metadata = {
  title: "Mckay's App Template",
  description: "A full-stack web app template."
}

export default async function RootLayout({
  children
}: {
  children: React.ReactNode
}) {
  return (
    <ClerkProvider>
      <html lang="en">
        <body>
          <Providers>
            <PostHogUserIdentify />
            <PostHogPageview />
            {children}
          </Providers>
        </body>
      </html>
    </ClerkProvider>
  )
}
```

## Diretório Components

O diretório `/components` contém componentes React reutilizáveis usando shadcn/ui.

### Estrutura

```plaintext
/components
  ├── ui/                  # Componentes básicos do shadcn
  │   ├── button.tsx
  │   ├── card.tsx
  │   └── dialog.tsx
  ├── layout/             # Componentes de layout
  │   ├── header.tsx
  │   └── footer.tsx
  └── utilities/          # Componentes utilitários
```

### Exemplo de Componente

```typescript
// components/ui/button.tsx
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

const buttonVariants = cva(
  "ring-offset-background focus-visible:ring-ring inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive: "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline: "border-input bg-background hover:bg-accent hover:text-accent-foreground border",
        secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline"
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
        icon: "size-10"
      }
    }
  }
)
```

## Diretório Database

O diretório `/db` contém schemas, migrações e queries usando Drizzle ORM.

### Estrutura

```plaintext
/db
  ├── schema.ts           # Definições de schema
  ├── migrations/         # Arquivos de migração
  └── types/             # Tipos TypeScript gerados
```

### Exemplo de Schema

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
```

### Configuração do Drizzle

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

## Diretório Hooks

O diretório `/hooks` contém custom React hooks reutilizáveis.

### Exemplo de Hooks

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

// hooks/use-local-storage.ts
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

## Diretório Library

O diretório `/lib` contém funções utilitárias e configurações.

### Estrutura

```plaintext
/lib
  ├── utils.ts           # Funções utilitárias gerais
  ├── db.ts             # Configuração do banco de dados
  ├── auth.ts           # Utilitários de autenticação
  └── api/              # Utilitários de API
```

### Exemplo de Utilitários

```typescript
// lib/utils.ts
import { type ClassValue, clsx } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

// lib/db.ts
import { drizzle } from "drizzle-orm/postgres-js"
import postgres from "postgres"

const connectionString = process.env.DATABASE_URL!
const client = postgres(connectionString)
export const db = drizzle(client)
```

## Diretório Prompts

O diretório `/prompts` contém templates e configurações para integração com IA.

### Estrutura

```plaintext
/prompts
  ├── templates/         # Templates de prompts reutilizáveis
  ├── config/           # Configurações de IA
  └── utils/            # Utilitários para prompts
```

### Exemplo de Template

```typescript
// prompts/templates/general.ts
export const promptTemplates = {
  summarize: `
    Summarize the following text:
    {text}
    
    Key points to include:
    - Main topic
    - Key arguments
    - Conclusions
  `,
  
  analyze: `
    Analyze the following content:
    {content}
    
    Consider:
    - Sentiment
    - Key themes
    - Notable patterns
  `
}
```

## Diretório Types

O diretório `/types` contém definições de tipos TypeScript.

### Estrutura

```plaintext
/types
  ├── index.ts          # Exportações de tipos principais
  ├── api/              # Tipos relacionados à API
  ├── db/               # Tipos do banco de dados
  └── components/       # Tipos dos componentes
```

### Exemplo de Tipos

```typescript
// types/index.ts
export interface User {
  id: string
  email: string
  name?: string
  role: "user" | "admin"
  createdAt: Date
  updatedAt: Date
}

export type Nullable<T> = T | null
export type Optional<T> = T | undefined

// types/api/index.ts
export interface APIResponse<T = any> {
  success: boolean
  data?: T
  error?: {
    code: string
    message: string
  }
}

// types/components/button.ts
export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: "default" | "primary" | "secondary" | "ghost"
  size?: "sm" | "md" | "lg"
  isLoading?: boolean
  leftIcon?: React.ReactNode
  rightIcon?: React.ReactNode
}
```

## Iniciando o Projeto

1. Clone o repositório
2. Copie `.env.example` para `.env.local` e preencha as variáveis de ambiente
3. Instale as dependências com `npm install`
4. Inicie o servidor de desenvolvimento com `npm run dev`
