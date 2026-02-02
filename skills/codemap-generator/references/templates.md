# Codemap Templates

Complete templates for generating codemaps.

## INDEX.md Template

```markdown
# Codebase Architecture

**Last Updated:** YYYY-MM-DD
**Repository:** project-name

## Overview

Brief description of the project architecture.

## Architecture Diagram

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Frontend   │────▶│   Backend   │────▶│  Database   │
└─────────────┘     └─────────────┘     └─────────────┘
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│   Assets    │     │  External   │
│             │     │  Services   │
└─────────────┘     └─────────────┘
```

## Areas

| Area | Description | Codemap |
|------|-------------|---------|
| Frontend | UI components, pages | [frontend.md](frontend.md) |
| Backend | API routes, services | [backend.md](backend.md) |
| Database | Schema, models | [database.md](database.md) |
| Integrations | External APIs | [integrations.md](integrations.md) |
| Workers | Background jobs | [workers.md](workers.md) |

## Entry Points

- `src/app/layout.tsx` - Main app entry
- `src/app/api/*` - API routes
- `src/workers/*` - Background workers

## Key Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| next | 15.x | Framework |
| react | 19.x | UI library |
| prisma | 5.x | ORM |
```

---

## Frontend Codemap Template

```markdown
# Frontend Architecture

**Last Updated:** YYYY-MM-DD
**Framework:** Next.js 15.x (App Router)
**Entry Point:** src/app/layout.tsx

## Structure

```
src/
├── app/                # Next.js App Router
│   ├── api/           # API routes
│   ├── (routes)/      # Page routes
│   └── layout.tsx     # Root layout
├── components/        # React components
│   ├── ui/           # Base UI components
│   └── features/     # Feature components
├── hooks/             # Custom React hooks
├── lib/               # Utilities
└── styles/            # Global styles
```

## Key Components

| Component | Purpose | Location |
|-----------|---------|----------|
| Layout | Root layout wrapper | app/layout.tsx |
| Header | Navigation header | components/Header.tsx |
| Footer | Page footer | components/Footer.tsx |

## Pages/Routes

| Route | Component | Purpose |
|-------|-----------|---------|
| / | page.tsx | Home page |
| /dashboard | dashboard/page.tsx | User dashboard |
| /api/health | api/health/route.ts | Health check |

## Data Flow

```
User Action
    │
    ▼
Component (useState/useContext)
    │
    ▼
Custom Hook (useSWR/useQuery)
    │
    ▼
API Route (/api/*)
    │
    ▼
Response → UI Update
```

## External Dependencies

- next - Framework
- react - UI library
- tailwindcss - Styling
- swr - Data fetching

## Related Areas

- [Backend](backend.md) - API implementation
- [Database](database.md) - Data models
```

---

## Backend Codemap Template

```markdown
# Backend Architecture

**Last Updated:** YYYY-MM-DD
**Runtime:** Next.js API Routes / FastAPI
**Entry Point:** src/app/api/ or src/main.py

## Structure

```
src/
├── app/api/           # API routes (Next.js)
│   ├── auth/         # Authentication
│   ├── users/        # User management
│   └── [resource]/   # Resource endpoints
├── services/          # Business logic
├── repositories/      # Data access
└── middleware/        # Request processing
```

## API Routes

| Route | Method | Purpose | Auth |
|-------|--------|---------|------|
| /api/health | GET | Health check | No |
| /api/auth/login | POST | User login | No |
| /api/users | GET | List users | Yes |
| /api/users/[id] | GET | Get user | Yes |

## Services

| Service | Purpose | Dependencies |
|---------|---------|--------------|
| AuthService | Authentication logic | UserRepository |
| UserService | User management | UserRepository, EmailService |

## Data Flow

```
Request
    │
    ▼
Middleware (auth, validation)
    │
    ▼
Route Handler
    │
    ▼
Service (business logic)
    │
    ▼
Repository (data access)
    │
    ▼
Database
    │
    ▼
Response
```

## Error Handling

| Error Code | Meaning | Response |
|------------|---------|----------|
| 400 | Bad Request | Validation errors |
| 401 | Unauthorized | Auth required |
| 404 | Not Found | Resource missing |
| 500 | Server Error | Internal error |

## External Dependencies

- Database client (Prisma/Supabase)
- Cache (Redis)
- Queue (BullMQ)

## Related Areas

- [Frontend](frontend.md) - API consumers
- [Database](database.md) - Data storage
- [Workers](workers.md) - Background jobs
```

---

## Integrations Codemap Template

```markdown
# External Integrations

**Last Updated:** YYYY-MM-DD

## Overview

```
┌──────────────┐
│   Our App    │
└──────┬───────┘
       │
   ┌───┴───┬───────┬───────┐
   ▼       ▼       ▼       ▼
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│Auth │ │ DB  │ │Cache│ │Email│
└─────┘ └─────┘ └─────┘ └─────┘
```

## Authentication

**Provider:** Auth0 / Clerk / Privy
**Location:** src/lib/auth/

| Feature | Implementation |
|---------|----------------|
| Login | OAuth 2.0 flow |
| Session | JWT tokens |
| Roles | RBAC via claims |

## Database

**Provider:** PostgreSQL (Supabase/Neon)
**ORM:** Prisma

| Table | Purpose |
|-------|---------|
| users | User accounts |
| posts | Content |
| comments | User comments |

## Cache

**Provider:** Redis / Upstash
**Location:** src/lib/cache/

| Key Pattern | TTL | Purpose |
|-------------|-----|---------|
| user:{id} | 1h | User profile |
| session:{token} | 24h | Session data |

## External APIs

| Service | Purpose | Rate Limit |
|---------|---------|------------|
| OpenAI | Embeddings | 3000/min |
| Stripe | Payments | 100/sec |
| SendGrid | Email | 100/sec |

## Environment Variables

| Variable | Service | Required |
|----------|---------|----------|
| DATABASE_URL | PostgreSQL | Yes |
| REDIS_URL | Redis | Yes |
| OPENAI_API_KEY | OpenAI | Yes |

## Related Areas

- [Backend](backend.md) - Integration usage
- [Workers](workers.md) - Async processing
```

---

## Database Codemap Template

```markdown
# Database Architecture

**Last Updated:** YYYY-MM-DD
**Database:** PostgreSQL
**ORM:** Prisma / Drizzle

## Schema Overview

```
┌──────────┐     ┌──────────┐
│  users   │────<│  posts   │
└──────────┘     └──────────┘
     │                │
     │           ┌────┴────┐
     │           ▼         ▼
     │      ┌────────┐ ┌────────┐
     └─────>│comments│ │  tags  │
            └────────┘ └────────┘
```

## Tables

### users
| Column | Type | Constraints |
|--------|------|-------------|
| id | uuid | PK |
| email | varchar | UNIQUE, NOT NULL |
| name | varchar | NOT NULL |
| created_at | timestamp | DEFAULT now() |

### posts
| Column | Type | Constraints |
|--------|------|-------------|
| id | uuid | PK |
| user_id | uuid | FK -> users.id |
| title | varchar | NOT NULL |
| content | text | |
| published | boolean | DEFAULT false |

## Indexes

| Table | Index | Columns | Type |
|-------|-------|---------|------|
| users | users_email_idx | email | UNIQUE |
| posts | posts_user_idx | user_id | BTREE |
| posts | posts_search_idx | title, content | GIN |

## Migrations

Location: `prisma/migrations/` or `drizzle/migrations/`

## Queries

Common query patterns:
- Get user with posts: `findUnique` with `include`
- Search posts: Full-text search with `to_tsvector`
- Paginated list: Cursor-based pagination

## Related Areas

- [Backend](backend.md) - Query execution
- [Integrations](integrations.md) - Database provider
```
