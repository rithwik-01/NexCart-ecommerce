# NexCart Ecommerce

A modern, full-stack ecommerce storefront built with Next.js 15, Sanity CMS, Clerk authentication, and Stripe payments.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router, Turbopack) |
| Language | TypeScript |
| CMS | Sanity v3 |
| Auth | Clerk |
| Payments | Stripe |
| State | Zustand (persisted) |
| Styling | Tailwind CSS v4 + shadcn/ui |
| Animations | Motion (Framer Motion) |
| Notifications | React Hot Toast |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Browser                              │
│                                                             │
│   ┌──────────┐   ┌──────────┐   ┌───────────────────────┐  │
│   │  Pages   │   │Components│   │  Zustand Store        │  │
│   │  /app    │◄──│/components│  │  - Cart (persisted)   │  │
│   │          │   │          │   │  - Favorites          │  │
│   └────┬─────┘   └──────────┘   └───────────────────────┘  │
└────────┼────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│                    Next.js Server                           │
│                                                             │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐   │
│   │ Server       │   │  API Routes  │   │  Middleware   │   │
│   │ Actions      │   │  /api/stripe │   │  (Clerk auth) │   │
│   │ /actions     │   │  /api/webhook│   └──────────────┘   │
│   └──────┬───────┘   └──────┬───────┘                      │
└──────────┼──────────────────┼──────────────────────────────┘
           │                  │
     ┌─────▼──────┐    ┌──────▼──────┐
     │  Sanity    │    │   Stripe    │
     │  (CMS)     │    │  (Payments) │
     │            │    │             │
     │  Products  │    │  Checkout   │
     │  Categories│    │  Webhooks   │
     │  Orders    │    │             │
     └────────────┘    └─────────────┘
           │
     ┌─────▼──────┐
     │   Clerk    │
     │   (Auth)   │
     │            │
     │  Sign In   │
     │  Sign Up   │
     │  Sessions  │
     └────────────┘
```

---

## Features

- **Product Browsing** — Browse products by category with search and filtering
- **Product Detail** — Full product pages with image carousel and variant selection
- **Cart** — Add, remove, and update quantities; cart persists across sessions via `localStorage`
- **Wishlist / Favorites** — Save products for later, also persisted locally
- **Authentication** — Sign up, sign in, and protected routes via Clerk middleware
- **Checkout** — Stripe-powered checkout with webhook handling for order confirmation
- **CMS** — All products, categories, and order records managed through Sanity Studio (`/studio`)
- **Discount Pricing** — Per-product discount percentage with automatic price calculation

---

## Project Structure

```
NexCart-ecommerce/
├── app/                  # Next.js App Router pages & API routes
│   ├── (store)/          # Storefront pages (home, product, cart, etc.)
│   ├── studio/           # Embedded Sanity Studio
│   └── api/              # Stripe webhook + checkout API routes
├── actions/              # Next.js Server Actions
├── components/           # Reusable React components
├── constants/            # App-wide constants (categories, nav links, etc.)
├── hooks/                # Custom React hooks
├── lib/                  # Utility functions & Sanity client
├── sanity/               # Sanity schemas (product, category, order)
├── store.ts              # Zustand global store (cart + favorites)
├── middleware.ts         # Clerk auth middleware
└── sanity.config.ts      # Sanity Studio configuration
```

---

## Getting Started

### Prerequisites

- Node.js 18+

### Installation

```bash
git clone https://github.com/rithwik-01/NexCart-ecommerce.git
npm install
```

### Environment Variables

Create a `.env.local` file in the root:

```env
# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=

# Sanity
NEXT_PUBLIC_SANITY_PROJECT_ID=
NEXT_PUBLIC_SANITY_DATASET=
SANITY_API_TOKEN=

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

### Seed the Database

A `seed.tar.gz` is included with sample product data. Import it into your Sanity project:

```bash
tar -xzf seed.tar.gz
npx sanity dataset import production.tar.gz production
```

### Run Locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to view the storefront.
Sanity Studio is available at [http://localhost:3000/studio](http://localhost:3000/studio).

---

## Deployment

Deploy to [Vercel](https://vercel.com) with one click:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/rithwik-01/NexCart-ecommerce)

After deploying, add your environment variables in the Vercel dashboard and configure your Stripe webhook endpoint to `https://your-domain.com/api/webhook`.
