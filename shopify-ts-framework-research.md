# Shopify Embedded App — TypeScript Framework Research

**Goal:** Best TypeScript framework for scalable Shopify embedded apps with a unified FE + BE stack.

---

## Recommendation: Remix + Shopify App Remix Stack

**Use Remix.** It is Shopify's officially backed framework, ships production-ready integrations out of the box, and uses TypeScript end-to-end in a single codebase.

---

## Why Remix Wins

### 1. Official Shopify Backing
Shopify ships and maintains `@shopify/shopify-app-remix`, their first-party Node.js package that handles everything Shopify-specific:
- OAuth + session token authentication (no cookies required — embedded-safe)
- Webhook registration and verification
- App Bridge v4 integration (mandatory since March 2024)
- Shopify Admin GraphQL API client per-store

The Shopify CLI scaffolds a Remix app with all of this pre-wired. No manual plumbing.

### 2. Unified TypeScript Codebase
Remix runs on Node.js and uses React for the frontend. Both live in the same project:
- **Backend**: `loader` and `action` functions (server-only, TypeScript)
- **Frontend**: React components + Polaris UI (TypeScript)
- **Shared types**: Define once, use everywhere — no API contract drift

### 3. Scalability Built-In
- **Session storage**: Prisma adapter — swap SQLite (dev) for PostgreSQL (prod) with one config line
- **Multi-tenancy**: Every Shopify store is an isolated session; the Remix package enforces scoping automatically
- **Webhooks**: Async-safe; offload long jobs to a queue
- **Performance**: Remix's streaming SSR keeps TTFB low at scale

### 4. GraphQL by Default
Shopify deprecated the REST API in October 2024. All new apps must use GraphQL Admin API. The Remix package ships with a pre-authenticated GraphQL client per request — zero setup.

---

## Recommended Stack

| Layer | Technology | Reason |
|---|---|---|
| Framework | **Remix** | Official Shopify support, unified FE+BE |
| Language | **TypeScript** | End-to-end type safety |
| UI | **@shopify/polaris** | Required for embedded app UX compliance |
| Embedding | **App Bridge v4** | Mandatory since March 2024 |
| ORM | **Prisma** | Type-safe, ships in Shopify template |
| DB (dev) | SQLite | Zero-config local dev |
| DB (prod) | **PostgreSQL** | Scalable, Prisma-compatible |
| API | **GraphQL** (Admin API) | REST deprecated Oct 2024 |
| Shopify SDK | `@shopify/shopify-app-remix` | First-party session, auth, webhooks |

---

## Why Not the Alternatives

| Option | Verdict |
|---|---|
| **Next.js** | No official Shopify package; requires manual OAuth, session, and App Bridge wiring. Only viable if the team has strong Next.js expertise and accepts the extra setup cost. |
| **T3 Stack (Next.js + tRPC + Prisma)** | Excellent type safety but community-only Shopify templates; adds tRPC learning curve with no advantage over Remix's native loaders/actions. |
| **NestJS + Next.js** | Overengineered — two runtimes, two build pipelines, increased ops complexity. Reserve only for microservice-scale products. |

---

## Getting Started

```bash
npm install -g @shopify/cli
shopify app create --template=remix
```

Out of the box you get: OAuth, App Bridge v4, Polaris, Prisma + SQLite session store, webhook handler, and TypeScript throughout.

For production, update `prisma/schema.prisma` datasource to PostgreSQL and set `DATABASE_URL`.

---

## Deployment

| Platform | Notes |
|---|---|
| **Fly.io** | Recommended; persistent Node.js, easy PostgreSQL add-on |
| **Railway** | One-click PostgreSQL, minimal config |
| **Vercel** | Works but requires serverless-aware session storage |

---

## Summary

**Remix + `@shopify/shopify-app-remix` + Prisma + Polaris** is the definitive TypeScript stack for Shopify embedded apps in 2025. Officially maintained by Shopify, it eliminates the most common integration pain points, enforces multi-tenancy patterns, and keeps frontend and backend in a single TypeScript codebase — directly satisfying the goal of a unified stack with reduced complexity.
