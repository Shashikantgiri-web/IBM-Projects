# 03 — System Architecture
## Employee Performance Intelligence System (EPIS)

---

## 1. Architecture Overview

EPIS follows a modern JAMstack architecture with server-side rendering via Next.js, Supabase as the backend-as-a-service layer, and PostgreSQL as the single source of truth.

```
                        ┌─────────────────────────────┐
                        │      USER (Browser)          │
                        └────────────┬────────────────┘
                                     │ HTTPS
                        ┌────────────▼────────────────┐
                        │        Vercel CDN            │
                        │    (Next.js 14 App)          │
                        └────────────┬────────────────┘
                                     │
              ┌──────────────────────┼───────────────────┐
              │                      │                   │
   ┌──────────▼───────┐  ┌──────────▼──────┐  ┌────────▼────────┐
   │   Page Routes     │  │   API Routes    │  │   Middleware    │
   │  /ceo /manager   │  │  /api/employees │  │  JWT + Role     │
   │  /employee       │  │  /api/charts    │  │  Guard          │
   └──────────────────┘  └──────────┬──────┘  └─────────────────┘
                                     │
                        ┌────────────▼────────────────┐
                        │         Supabase             │
                        ├─────────────────────────────┤
                        │  Auth (JWT + Sessions)       │
                        │  PostgreSQL Database         │
                        │  Row Level Security (RLS)    │
                        │  Storage (if needed)         │
                        └────────────┬────────────────┘
                                     │
                        ┌────────────▼────────────────┐
                        │   PostgreSQL Tables          │
                        │   employees, departments,    │
                        │   performance, training,     │
                        │   salary, users, roles       │
                        └─────────────────────────────┘
```

---

## 2. Data Pipeline (Completed)

The data pipeline was completed by the Data Analysis and Data Engineering teams before web development began.

```
Raw IBM HR Dataset (Excel)
          │
          ▼
Data Analysis Team
  • Excel cleaning and validation
  • Business rule definition
  • KPI and metric definition
  • Power BI dashboard creation
  • DAX measure writing
          │
          ▼
Final Analytical Dataset (Excel / CSV)
          │
          ▼
Python ETL Pipeline
  • Data transformation
  • Column normalization
  • Relationship mapping
  • Data quality checks
          │
          ▼
Supabase PostgreSQL Database  ← Web app reads from here
```

**Important:** Power BI reads directly from Supabase. The web application also reads from Supabase. Both use the same database. Power BI is NOT part of the web application.

---

## 3. Authentication Flow

```
User visits /login
      │
      ▼
Enter email + password
      │
      ▼
POST to Supabase Auth
      │
      ├── Invalid → Show error message
      │
      └── Valid →
            JWT token issued
                  │
                  ▼
            Role fetched from users table
                  │
                  ▼
            Middleware reads role from JWT
                  │
                  ├── CEO     → redirect /dashboard/ceo
                  ├── Manager → redirect /dashboard/manager
                  ├── Employee→ redirect /dashboard/employee
                  └── Tester  → redirect /dashboard/ceo (read-only)
```

---

## 4. Request Flow (API)

```
React Component
      │
      ▼ fetch / useQuery
API Route (/api/*)
      │
      ▼
Verify JWT (Supabase auth.getUser)
      │
      ├── Invalid token → 401 Unauthorized
      │
      └── Valid token →
            Check user role
                  │
                  ▼
            Supabase DB Query (with RLS enforced)
                  │
                  ▼
            Return JSON response
                  │
                  ▼
            Component renders chart / table
```

---

## 5. Frontend Architecture

```
Next.js App Router
│
├── app/
│   ├── (auth)/           ← Public routes — no auth required
│   │   ├── login/
│   │   ├── signup/
│   │   └── forgot-password/
│   │
│   ├── (dashboard)/      ← Protected routes — auth + role required
│   │   ├── ceo/
│   │   │   ├── page.tsx           ← CEO Overview
│   │   │   ├── employees/page.tsx ← Employee list
│   │   │   ├── departments/page.tsx
│   │   │   └── reports/page.tsx
│   │   │
│   │   ├── manager/
│   │   │   ├── page.tsx
│   │   │   ├── employees/page.tsx
│   │   │   └── performance/page.tsx
│   │   │
│   │   └── employee/
│   │       ├── page.tsx
│   │       └── profile/page.tsx
│   │
│   ├── about/page.tsx
│   ├── settings/page.tsx
│   ├── profile/page.tsx
│   ├── unauthorized/page.tsx
│   └── not-found.tsx
│
├── middleware.ts          ← Route protection logic
└── layout.tsx            ← Root layout with providers
```

---

## 6. Component Architecture Overview

```
Layout
├── Navbar
│   ├── Logo
│   ├── NavigationLinks
│   ├── ThemeToggle
│   └── UserMenu
│
├── Sidebar (dashboard only)
│   ├── SidebarNav
│   └── SidebarItem
│
└── Main Content
    ├── PageHeader
    ├── KPICardGrid
    │   └── KPICard (×n)
    ├── ChartGrid
    │   ├── BarChart
    │   ├── LineChart
    │   ├── PieChart
    │   └── AreaChart
    ├── DataTable
    │   ├── TableFilters
    │   ├── TableSearch
    │   └── TablePagination
    └── Footer
```

---

## 7. State Management

| State | Managed By | Stored In |
|---|---|---|
| Authentication session | Supabase Auth | Browser session |
| User role | Supabase JWT | JWT claims |
| Theme (dark/light) | React Context | localStorage |
| Active department filter | React useState | Component |
| Active employee filter | React useState | Component |
| Chart data | React Query / SWR | In-memory cache |
| Loading states | React useState | Component |
| Error states | React useState | Component |

---

## 8. Security Architecture

| Layer | Security Mechanism |
|---|---|
| Route Level | Next.js middleware verifies JWT before any dashboard page loads |
| API Level | Every /api/* route calls supabase.auth.getUser() before querying |
| Database Level | Supabase RLS policies filter rows by user role and ID |
| Transport | HTTPS enforced via Vercel |
| Environment | Secrets stored in .env.local, never committed to Git |
| Frontend | React JSX prevents XSS by escaping all values |

---

## 9. Deployment Architecture

```
Developer Machine
      │
      ▼ git push origin main
GitHub Repository
      │
      ▼ Automatic deploy trigger
Vercel Build Pipeline
  • next build
  • Environment variables injected
  • Static assets to CDN
      │
      ▼
Vercel Production
  • epis.vercel.app (or custom domain)
  • Serverless API routes
  • Global CDN for static assets
      │
      ▼
Supabase (separate hosted service)
  • PostgreSQL database
  • Auth service
  • RLS policies active
```

---

## 10. Environment Variables Required

```
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key (server-side only)
NEXT_PUBLIC_APP_URL=https://your-app.vercel.app
```
