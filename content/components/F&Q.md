[[ORBIT-EO ideas]]

You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure  
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles. 
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
faqs-02.tsx
import { ArrowRight, Clock, Mail, MessageCircle } from "lucide-react";

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { Button } from "@/components/ui/button";

const faqs = [
  {
    q: "What's included in Pro?",
    a: "All 135+ blocks, every category, lifetime updates and registry access from /account/registry-token.",
  },
  {
    q: "Can my team share one license?",
    a: "Pro is per-seat. Team plans include up to 10 seats with a per-user token each.",
  },
  {
    q: "What stack is required?",
    a: "Next.js (App Router) with React 19, Tailwind v4 and shadcn/ui. Older versions might work but aren't officially supported.",
  },
  {
    q: "Are blocks responsive?",
    a: "Every block reads from ~380px to full desktop. Mobile drawers and stacked layouts ship with each section.",
  },
  {
    q: "Do you handle dark mode?",
    a: "Yes — every block uses shadcn CSS variables so it adapts to either theme out of the box.",
  },
];

export default function Faqs02() {
  return (
    <section className="bg-background py-20 sm:py-28">
      <div className="mx-auto max-w-6xl px-6 sm:px-10">
        <div className="grid gap-10 lg:grid-cols-[1fr_1.4fr] lg:items-start">
          <div className="lg:sticky lg:top-24 flex flex-col gap-5">
            <span className="inline-flex w-fit items-center gap-1.5 rounded-full border border-border bg-card px-3 py-1 text-xs font-medium text-muted-foreground shadow-sm shadow-black/5">
              FAQs
            </span>
            <h2
              className="text-balance font-semibold tracking-tight"
              style={{
                fontSize: "clamp(1.85rem, 4vw, 2.75rem)",
                letterSpacing: "-0.03em",
                lineHeight: 1.08,
              }}
            >
              Your questions answered
            </h2>
            <p className="text-sm text-muted-foreground">
              Quick answers from the team. Still stuck? Write to{" "}
              <a
                href="#"
                className="font-medium text-foreground underline-offset-4 hover:underline"
              >
                hey@blockus.dev
              </a>{" "}
              and a real person replies.
            </p>

            <div className="mt-2 flex flex-col gap-2 rounded-2xl border border-border bg-card p-4">
              <div className="flex items-center gap-3">
                <span className="grid size-9 shrink-0 place-items-center rounded-full bg-foreground text-background">
                  <MessageCircle className="size-4" />
                </span>
                <div className="flex flex-col leading-tight">
                  <p className="text-sm font-medium text-foreground">
                    Talk to a human
                  </p>
                  <p className="text-xs text-muted-foreground">
                    <Clock className="mr-1.5 inline size-3" />
                    Replies in &lt; 4h
                  </p>
                </div>
              </div>
              <Button size="sm" className="mt-2 w-full rounded-full">
                Open a ticket
                <ArrowRight />
              </Button>
              <p className="text-center text-[11px] text-muted-foreground">
                <Mail className="mr-1 inline size-3" />
                Or email{" "}
                <a
                  href="mailto:hey@blockus.dev"
                  className="font-medium text-foreground underline-offset-4 hover:underline"
                >
                  hey@blockus.dev
                </a>
              </p>
            </div>
          </div>

          <Accordion type="single" collapsible>
            {faqs.map((f, i) => (
              <AccordionItem key={f.q} value={`item-${i}`}>
                <AccordionTrigger>{f.q}</AccordionTrigger>
                <AccordionContent>{f.a}</AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
    </section>
  );
}


demo.tsx
import Faqs02 from "@/components/ui/faqs-02";

export default function Faqs02Demo() {
  return <Faqs02 />;
}

```

Copy-paste these files for dependencies:
```tsx
shadcn/accordion
"use client"

import * as React from "react"
import * as AccordionPrimitive from "@radix-ui/react-accordion"
import { ChevronDown } from "lucide-react"

import { cn } from "@/lib/utils"

const Accordion = AccordionPrimitive.Root

const AccordionItem = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Item>
>(({ className, ...props }, ref) => (
  <AccordionPrimitive.Item
    ref={ref}
    className={cn("border-b", className)}
    {...props}
  />
))
AccordionItem.displayName = "AccordionItem"

const AccordionTrigger = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Trigger>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Trigger>
>(({ className, children, ...props }, ref) => (
  <AccordionPrimitive.Header className="flex">
    <AccordionPrimitive.Trigger
      ref={ref}
      className={cn(
        "flex flex-1 items-center justify-between py-4 font-medium transition-all hover:underline [&[data-state=open]>svg]:rotate-180",
        className,
      )}
      {...props}
    >
      {children}
      <ChevronDown className="h-4 w-4 shrink-0 transition-transform duration-200" />
    </AccordionPrimitive.Trigger>
  </AccordionPrimitive.Header>
))
AccordionTrigger.displayName = AccordionPrimitive.Trigger.displayName

const AccordionContent = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Content>
>(({ className, children, ...props }, ref) => (
  <AccordionPrimitive.Content
    ref={ref}
    className="overflow-hidden text-sm transition-all data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down"
    {...props}
  >
    <div className={cn("pb-4 pt-0", className)}>{children}</div>
  </AccordionPrimitive.Content>
))

AccordionContent.displayName = AccordionPrimitive.Content.displayName

export { Accordion, AccordionItem, AccordionTrigger, AccordionContent }

```
```tsx
shadcn/button
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline:
          "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  },
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
  },
)
Button.displayName = "Button"

export { Button, buttonVariants }

```

Install NPM dependencies:
```bash
lucide-react, @radix-ui/react-accordion, @radix-ui/react-slot, class-variance-authority
```

https://21st.dev/@ln-dev7/components/faqs-02

![[Screenshot 2026-10-19 at 7.33.52 PM.png]]