# Project Memory
Last updated: 2025-04-08 | Session 7 | Branch: feature/checkout
Memory health: 8/10

## Project Overview
E-commerce site for a local bakery ("Sweet Crumb"). Next.js 14 App Router,
Prisma + PostgreSQL, Stripe payments, Tailwind CSS. Deployed on Vercel,
DB on Neon. Target launch: end of April.

## Where We Left Off
- **Current task:** Stripe webhook handler in `src/app/api/webhooks/stripe/route.ts`
- **Status:** in progress — `handlePaymentSuccess()` is complete, `handleRefund()` is stubbed
- **Next immediate step:** Implement `handleRefund()` using the Stripe refund event payload, then write integration tests for both webhook handlers
- **Open question:** Use Stripe CLI for local webhook testing or mock the events?

## Completed
- [Sessions 1-3] Project scaffolding, Prisma schema (User, Product, Order, OrderItem), seed data
- [2025-03-28] Product listing page with SSR + pagination (`src/app/products/page.tsx`)
- [2025-03-30] Shopping cart with Zustand store + localStorage persistence
- [2025-04-02] Cart page UI with quantity controls and price calculation
- [2025-04-05] Stripe checkout session creation in `src/app/api/checkout/route.ts`
- [2025-04-07] `handlePaymentSuccess()` webhook — creates Order, sends confirmation email via Resend

## Active Work
- [ ] `handleRefund()` in webhook handler (next up)
- [ ] Integration tests for webhook endpoints
- [ ] Order confirmation email template (Resend + React Email)
- [ ] Admin dashboard — order list with status filters (not started)
- [ ] Product image optimization with next/image (not started)

## Blockers
- Need Stripe webhook signing secret from client's Stripe dashboard before
  testing payments end-to-end in staging. Dev testing works with Stripe CLI.

## Key Decisions
| Date | Decision | Reasoning | Affects |
|------|----------|-----------|---------|
| 2025-03-20 | App Router over Pages Router | Server components, better data fetching, future-proof | All pages |
| 2025-03-20 | Prisma over Drizzle | Better TypeScript inference, built-in migrations, team familiarity | All DB access |
| 2025-03-25 | Stripe over Paddle | Client already has Stripe account, better API docs | Payment flow |
| 2025-03-25 | Zustand over Context for cart | Simpler API, localStorage middleware built-in | Cart state |
| 2025-04-02 | Resend over SendGrid for email | Better DX, React Email support, simpler pricing | Transactional email |

## Key Files
| File | Purpose | Last Modified |
|------|---------|---------------|
| `src/app/api/webhooks/stripe/route.ts` | Stripe webhook handler | Session 7 |
| `src/app/api/checkout/route.ts` | Creates Stripe checkout sessions | Session 6 |
| `src/lib/stripe.ts` | Stripe client singleton + helpers | Session 5 |
| `src/stores/cart.ts` | Zustand cart store with persistence | Session 4 |
| `src/app/products/page.tsx` | Product listing with pagination | Session 3 |
| `prisma/schema.prisma` | Database schema | Session 3 |
| `src/lib/email.ts` | Resend client + email helpers | Session 7 |

## Architecture Notes
- All payment logic is server-side only. No Stripe keys in client code.
- Cart is client-side (Zustand + localStorage). On checkout, cart items are
  sent to `/api/checkout` which validates against DB prices (never trust client).
- Webhook handler verifies Stripe signature before processing any event.
- Orders are created only after successful payment confirmation via webhook,
  not at checkout session creation.

## Known Issues
- Product images are unoptimized PNGs from the client. Need to convert to
  WebP and use next/image with proper sizing. Low priority until launch.
- Cart doesn't sync across tabs (localStorage limitation). Acceptable for now.

## Session Log
| Session | Date | Summary |
|---------|------|---------|
| 5 | 2025-04-02 | Cart page UI, started Stripe integration |
| 6 | 2025-04-05 | Stripe checkout session creation, tested with Stripe CLI |
| 7 | 2025-04-08 | Webhook handler (payment success), email setup with Resend |

## User Preferences
- Prefers server components unless interactivity is specifically needed
- Wants explicit error handling (no silent catches)
- Likes small, focused commits with conventional commit messages
- Prefers Zod for all runtime validation

## External Context
- Stripe account: client-managed (test mode keys in `.env.local`)
- Neon PostgreSQL: connection string in `DATABASE_URL`
- Resend: API key in `RESEND_API_KEY`, sending from `orders@sweetcrumb.com`
- Vercel deployment: auto-deploys from `main` branch
