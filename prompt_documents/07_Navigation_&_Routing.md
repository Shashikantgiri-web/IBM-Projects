# 07 — Navigation & Routing
## Employee Performance Intelligence System (EPIS)

---

## 1. Route Map

### 1.1 Public Routes (No Authentication Required)

| Route | Page | Component |
|---|---|---|
| `/` | Root redirect | Redirects to `/login` or dashboard based on auth |
| `/login` | Login Page | `app/(auth)/login/page.tsx` |
| `/forgot-password` | Forgot Password | `app/(auth)/forgot-password/page.tsx` |
| `/reset-password` | Reset Password | `app/(auth)/reset-password/page.tsx` |
| `/about` | About Page | `app/about/page.tsx` |

---

### 1.2 Protected Routes (Authentication Required)

| Route | Page | Allowed Roles |
|---|---|---|
| `/dashboard/ceo` | CEO Dashboard | `ceo`, `tester` |
| `/dashboard/ceo/employees` | All Employees List | `ceo`, `tester` |
| `/dashboard/ceo/departments` | Department Overview | `ceo`, `tester` |
| `/dashboard/ceo/reports` | Reports & Exports | `ceo`, `tester` |
| `/dashboard/manager` | Manager Dashboard | `manager`, `ceo`, `tester` |
| `/dashboard/manager/team` | Team Roster | `manager`, `ceo`, `tester` |
| `/dashboard/manager/analytics` | Team Analytics | `manager`, `ceo`, `tester` |
| `/dashboard/employee` | Employee Dashboard | `employee`, `manager`, `ceo`, `tester` |
| `/profile` | Profile Page | All authenticated |
| `/settings` | Settings Page | All authenticated |

---

### 1.3 Error Routes

| Route | Page |
|---|---|
| `/unauthorized` | 403 Unauthorized |
| `*` (catch-all) | 404 Not Found |

---

## 2. App Router Directory Structure

```
app/
├── (auth)/                         ← Auth layout group (no sidebar)
│   ├── layout.tsx                  ← Auth-specific layout
│   ├── login/
│   │   └── page.tsx
│   ├── forgot-password/
│   │   └── page.tsx
│   └── reset-password/
│       └── page.tsx
│
├── (dashboard)/                    ← Dashboard layout group (with sidebar)
│   ├── layout.tsx                  ← Dashboard layout (sidebar + topbar)
│   ├── dashboard/
│   │   ├── ceo/
│   │   │   ├── page.tsx            ← CEO Overview
│   │   │   ├── employees/
│   │   │   │   └── page.tsx
│   │   │   ├── departments/
│   │   │   │   └── page.tsx
│   │   │   └── reports/
│   │   │       └── page.tsx
│   │   ├── manager/
│   │   │   ├── page.tsx            ← Manager Overview
│   │   │   ├── team/
│   │   │   │   └── page.tsx
│   │   │   └── analytics/
│   │   │       └── page.tsx
│   │   └── employee/
│   │       └── page.tsx            ← Employee Overview
│   ├── profile/
│   │   └── page.tsx
│   └── settings/
│       └── page.tsx
│
├── about/
│   └── page.tsx
├── unauthorized/
│   └── page.tsx
├── not-found.tsx                   ← Global 404
├── error.tsx                       ← Global error boundary
├── layout.tsx                      ← Root layout
└── page.tsx                        ← Root page (redirect logic)
```

---

## 3. Middleware Configuration

**File:** `middleware.ts` (at project root)

```typescript
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export async function middleware(req: NextRequest) {
  const res = NextResponse.next()
  const supabase = createMiddlewareClient({ req, res })
  const { data: { session } } = await supabase.auth.getSession()

  const { pathname } = req.nextUrl

  // Allow public routes
  const publicRoutes = ['/login', '/forgot-password', '/reset-password', '/about']
  if (publicRoutes.includes(pathname)) {
    // If authenticated user tries to access login, redirect to their dashboard
    if (session) {
      return redirectToDashboard(session.user.user_metadata.role, req)
    }
    return res
  }

  // All other routes require authentication
  if (!session) {
    return NextResponse.redirect(new URL('/login', req.url))
  }

  const role = session.user.user_metadata.role

  // CEO routes
  if (pathname.startsWith('/dashboard/ceo') && !['ceo', 'tester'].includes(role)) {
    return NextResponse.redirect(new URL('/unauthorized', req.url))
  }

  // Manager routes
  if (pathname.startsWith('/dashboard/manager') && !['manager', 'ceo', 'tester'].includes(role)) {
    return NextResponse.redirect(new URL('/unauthorized', req.url))
  }

  return res
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico|public/).*)'],
}
```

---

## 4. Navigation Guards (Client-Side)

For additional protection in client components, a `useRequireRole` hook is provided:

```typescript
// hooks/useRequireRole.ts
import { useRouter } from 'next/navigation'
import { useEffect } from 'react'
import { useAuth } from '@/features/auth/useAuth'

export function useRequireRole(allowedRoles: string[]) {
  const { user, loading } = useAuth()
  const router = useRouter()

  useEffect(() => {
    if (!loading && !user) {
      router.push('/login')
    }
    if (!loading && user && !allowedRoles.includes(user.role)) {
      router.push('/unauthorized')
    }
  }, [user, loading, allowedRoles, router])
}
```

---

## 5. Sidebar Navigation Links

### CEO Navigation

```
├── Dashboard          → /dashboard/ceo
├── Employees          → /dashboard/ceo/employees
├── Departments        → /dashboard/ceo/departments
├── Reports            → /dashboard/ceo/reports
├── ─────────────────
├── Profile            → /profile
├── Settings           → /settings
└── Logout
```

### Manager Navigation

```
├── Dashboard          → /dashboard/manager
├── My Team            → /dashboard/manager/team
├── Analytics          → /dashboard/manager/analytics
├── ─────────────────
├── Profile            → /profile
├── Settings           → /settings
└── Logout
```

### Employee Navigation

```
├── My Dashboard       → /dashboard/employee
├── ─────────────────
├── Profile            → /profile
├── Settings           → /settings
└── Logout
```

---

## 6. Breadcrumb Logic

All dashboard sub-pages display a breadcrumb at the top:

| Current Page | Breadcrumb |
|---|---|
| `/dashboard/ceo` | Home > CEO Dashboard |
| `/dashboard/ceo/employees` | Home > CEO Dashboard > Employees |
| `/dashboard/manager/team` | Home > Manager Dashboard > My Team |
| `/profile` | Home > Profile |

---

## 7. Root Page Redirect Logic

`app/page.tsx` contains redirect logic based on auth state:

```
User visits /
      │
      ▼
Check Supabase session
      │
      ├── No session → redirect to /login
      │
      └── Has session → redirect to role dashboard
            ├── ceo → /dashboard/ceo
            ├── manager → /dashboard/manager
            ├── employee → /dashboard/employee
            └── tester → /dashboard/ceo
```

---

## 8. Active Link Highlighting

The sidebar uses `usePathname()` from Next.js to detect the current route and apply the active style:

```tsx
const pathname = usePathname()
const isActive = pathname === href || pathname.startsWith(href + '/')
```

Active item: left `4px` blue border + light blue background tint.
