# STAR Responses: BCG Platinion Principal Architect - AI Platforms
## Mapped to `cookbook-club` Repository Evidence
### Repository: `git@github.com:n8mauer/cookbook-club.git`
### Live: `https://cookbookclub.vip`

---

## STAR 7: Production AI-Integrated Full-Stack Platform

**JD Requirement:** *"Proficiency in Python, JavaScript, TypeScript, Node.js" / "Frontend frameworks (React, Next.js) for intelligent app interfaces" / "Understanding of cloud architecture, data management, and API-based system integration"*

**Situation:** Social cooking clubs lacked a digital platform to manage their workflow — coordinating families, scheduling gatherings, selecting recipes from shared cookbooks, tracking dietary restrictions, and generating shopping lists. This required a production-grade, multi-tenant application with AI capabilities integrated at multiple touchpoints.

**Task:** Design, build, and deploy a full-stack application with AI-powered features (OCR, smart shopping lists, allergen-safe substitutions, cookbook recommendations), real-time collaboration, multi-provider authentication, and a mobile companion — all serving live users at `cookbookclub.vip`.

**Action:**
- Built a **full-stack TypeScript monolith** with React 18 + Vite frontend, Express 5 backend, and PostgreSQL 16 on AWS RDS — with shared Zod/Drizzle schemas as the single source of truth across client and server.
- Designed a **22+ table relational schema** with Drizzle ORM covering families, members, cookbooks, recipes, meetings, time-slot voting, recipe selections, reviews, circles (social groups), connections (friend graph), badges, and AI cache — with full foreign key relationships, cascading deletes, and 20+ performance indexes.
- Integrated **OpenAI GPT-4o** at 7 distinct touchpoints: cookbook cover OCR, recipe photo OCR, allergen-safe ingredient substitution, multi-recipe shopping list aggregation with category organization, AI cookbook suggestions based on rating history, host-preference-weighted recommendations, and conversational AI.
- Implemented **SHA-256 hash-based AI response caching** in a dedicated `ai_cache` PostgreSQL table with configurable TTLs (7-30 days), eliminating redundant OpenAI API calls and reducing costs.
- Built **multi-provider authentication**: Google OAuth, Facebook OAuth, and Apple Sign-In for mobile — with dual auth strategy (server-side sessions via `connect-pg-simple` for web, HS256 JWT with 7-day access / 30-day refresh tokens for mobile).
- Deployed on **AWS** (EC2 + Nginx reverse proxy, RDS PostgreSQL, S3 for image storage with presigned URLs, SES for transactional email with DKIM, SNS for SMS notifications) with Cloudflare DNS.
- Implemented **React code-splitting** with `React.lazy` for 13 page components, shadcn/ui component library (40+ components), TanStack Query for server state, and a social graph visualization using `react-force-graph-2d`.
- Built a **React Native/Expo** mobile wrapper with EAS Build for iOS and Android distribution.
- Wrote comprehensive tests with **Vitest**: schema validation for all 14 Zod insert schemas, database index verification, JWT sign/verify tests, security header tests (CSP, X-Frame-Options), code-splitting enforcement tests, and Circle CRUD integration tests.

**Result:** A live production platform at `cookbookclub.vip` serving real users, demonstrating end-to-end ownership from architecture through deployment. The project showcases the ability to integrate AI capabilities (GPT-4o) as first-class features across a complex application — not as an afterthought, but woven into core workflows (OCR-powered data entry, allergen-aware meal planning, intelligent shopping lists). This mirrors the BCG Platinion mandate of embedding AI into business processes to deliver tangible user value, with the production deployment proving the approach works at scale.

---

## STAR 8: Enterprise API Design & Security Architecture

**JD Requirement:** *"Understanding of cloud architecture, data management, and API-based system integration" / "Integration of agent workflows with enterprise APIs, knowledge bases"*

**Situation:** The cookbook-club platform needed to integrate with 5+ external services (OpenAI, Google OAuth/Calendar/Maps, Facebook, Apple Sign-In, AWS SES/SNS/S3), handle complex authorization across multiple user roles and group types, and maintain security best practices — all while keeping the API surface manageable for both web and mobile clients.

**Task:** Design a REST API layer with 124+ endpoints that orchestrates external service integrations, enforces role-based access control, and follows enterprise security patterns.

**Action:**
- Designed a **Repository Pattern** with an `IStorage` interface defining 70+ storage methods, implemented by `DatabaseStorage` using Drizzle ORM — enabling clean separation of business logic from data access.
- Built a **dual authentication middleware** that transparently handles both session-based (web) and JWT Bearer token (mobile) auth paths in a single `isAuthenticated` middleware, with `isAdmin` role checks against a configurable allowlist.
- Implemented **Helmet.js security headers**: strict Content Security Policy, frame protection, `X-Content-Type-Options: nosniff`, no server fingerprinting — verified by automated integration tests.
- Designed **fallback chains** for resilience: email sending falls back from Gmail API to SES; auth falls back across providers; OpenAI client uses lazy initialization via JavaScript Proxy to avoid crashes when the API key is missing.
- Implemented **presigned URL pattern for S3**: client requests a time-limited PUT URL, uploads directly to S3, stores a proxy path for server-side streaming — keeping the S3 bucket fully private.
- Integrated **Google Calendar API** for meeting event creation, **Google Maps Directions API** for travel time calculations, and **Doodle-style time-slot voting** with RSVP tracking.

**Result:** A production API serving web and mobile clients with enterprise-grade security, multi-provider auth, and resilient external service integration. The architectural decisions — Repository Pattern, dual auth middleware, fallback chains, presigned uploads — demonstrate the kind of pragmatic, security-conscious API design that BCG Platinion architects implement for enterprise clients. The comprehensive test suite (schema validation, security header verification, integration tests) shows commitment to verifiable quality.

---

## Tech Stack Summary

| Layer | Technology |
|-------|-----------|
| **Language** | TypeScript (full-stack) |
| **Frontend** | React 18, Vite 7, Tailwind CSS, shadcn/ui (40+ components), TanStack Query v5, Framer Motion, Recharts, react-force-graph-2d |
| **Backend** | Express 5, Drizzle ORM, Zod validation, Helmet.js, Passport.js |
| **Database** | PostgreSQL 16 on AWS RDS (22+ tables, 20+ indexes) |
| **AI** | OpenAI GPT-4o (7 integration points), SHA-256 response caching |
| **Auth** | Google OAuth, Facebook OAuth, Apple Sign-In, JWT (mobile), Sessions (web) |
| **Cloud** | AWS EC2, RDS, S3, SES, SNS, Cloudflare DNS |
| **Mobile** | React Native / Expo SDK 52, EAS Build (iOS + Android) |
| **Testing** | Vitest (schema, security, integration, code-splitting tests) |
| **External APIs** | Google Calendar, Google Maps Directions, OpenAI, Facebook Graph |
