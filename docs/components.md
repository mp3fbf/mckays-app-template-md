# Components Directory

The `/components` directory contains all the reusable React components used throughout the application. The components are built using [shadcn/ui](https://ui.shadcn.com/) as the base component library, along with Tailwind CSS for styling.

## Structure

```plaintext
/components
  ├── ui/                  # Basic UI components from shadcn
  │   ├── button.tsx
  │   ├── card.tsx
  │   ├── dialog.tsx
  │   └── ...
  ├── layout/             # Layout components
  │   ├── header.tsx
  │   ├── footer.tsx
  │   └── ...
  ├── utilities/          # Utility components
  │   ├── providers.tsx
  │   └── ...
  └── shared/            # Shared components used across features
```

## UI Components

The UI components are based on shadcn/ui and customized for the application. Here's an example of the Button component:

```typescript
// components/ui/button.tsx
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

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
    },
    defaultVariants: {
      variant: "default",
      size: "default"
    }
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : "button"
    return (
      <Comp
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    )
  }
)
```

## Key Features

1. **Shadcn/UI Integration**
   - Uses shadcn/ui as the base component library
   - Customizable components with consistent styling
   - Built on top of Radix UI primitives

2. **Tailwind CSS**
   - Utility-first CSS framework
   - Custom variants and themes
   - Responsive design utilities

3. **Type Safety**
   - Full TypeScript support
   - Props validation
   - Component variants using class-variance-authority

4. **Accessibility**
   - Built with accessibility in mind
   - ARIA attributes and keyboard navigation
   - Focus management

## Utility Components

Special utility components include:

1. **Providers**
   - Theme provider for light/dark mode
   - Toast notifications
   - Modal/dialog context

2. **Layout Components**
   - Header and navigation
   - Footer
   - Sidebar
   - Page layouts

3. **Analytics Integration**
   - PostHog tracking components
   - User identification
   - Event tracking