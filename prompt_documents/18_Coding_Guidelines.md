# 18 — Coding Guidelines
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

These guidelines ensure that all code in EPIS is consistent, readable, and maintainable — whether written by one developer or a team. Every developer working on this project must follow these rules.

---

## 2. Folder Structure

```
epis/
├── app/                          ← Next.js App Router pages and API routes
│   ├── (auth)/                   ← Public auth pages (no layout wrapping)
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── forgot-password/
│   │       └── page.tsx
│   ├── (dashboard)/              ← Protected dashboard pages (share DashboardLayout)
│   │   ├── layout.tsx            ← DashboardLayout applied here
│   │   ├── ceo/
│   │   │   ├── page.tsx
│   │   │   ├── employees/
│   │   │   │   └── page.tsx
│   │   │   └── departments/
│   │   │       └── page.tsx
│   │   ├── manager/
│   │   │   └── page.tsx
│   │   └── employee/
│   │       └── page.tsx
│   ├── api/                      ← API route handlers
│   │   ├── dashboard/
│   │   │   └── kpis/
│   │   │       └── route.ts
│   │   ├── employees/
│   │   │   ├── route.ts
│   │   │   └── [id]/
│   │   │       └── route.ts
│   │   ├── charts/
│   │   │   ├── attrition-by-department/
│   │   │   │   └── route.ts
│   │   │   └── ...
│   │   └── export/
│   │       └── employees/
│   │           └── route.ts
│   ├── about/
│   │   └── page.tsx
│   ├── settings/
│   │   └── page.tsx
│   ├── unauthorized/
│   │   └── page.tsx
│   ├── not-found.tsx
│   ├── layout.tsx                ← Root layout: providers + fonts
│   └── globals.css
│
├── components/                   ← Reusable React components
│   ├── layout/
│   ├── kpi/
│   ├── charts/
│   ├── filters/
│   ├── table/
│   ├── forms/
│   ├── feedback/
│   └── shared/
│
├── context/                      ← React context providers
│   ├── AuthContext.tsx
│   └── ThemeContext.tsx
│
├── hooks/                        ← Custom React hooks (data fetching)
│   ├── useDashboardKPIs.ts
│   ├── useEmployees.ts
│   ├── useEmployee.ts
│   ├── useDepartments.ts
│   └── useChartData.ts
│
├── lib/                          ← Utility functions and shared logic
│   ├── supabase/
│   │   ├── client.ts             ← Browser Supabase client
│   │   └── server.ts             ← Server-side Supabase client
│   ├── utils.ts                  ← General helpers
│   ├── formatters.ts             ← Number/date formatting
│   └── constants.ts              ← App-wide constants
│
├── services/                     ← API call functions used by hooks
│   ├── employeeService.ts
│   ├── departmentService.ts
│   └── chartService.ts
│
├── types/                        ← TypeScript type definitions
│   ├── employee.ts
│   ├── department.ts
│   ├── chart.ts
│   ├── api.ts
│   └── auth.ts
│
├── utils/                        ← Pure utility functions
│   ├── csvExport.ts
│   ├── attritionCalculator.ts
│   └── queryBuilder.ts
│
├── middleware.ts                  ← Route protection middleware
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

---

## 3. Naming Conventions

### Files and Folders

| Item | Convention | Example |
|---|---|---|
| Page files | lowercase with hyphens | `forgot-password/page.tsx` |
| Component files | PascalCase | `KPICard.tsx` |
| Hook files | camelCase with `use` prefix | `useEmployees.ts` |
| Utility files | camelCase | `formatters.ts` |
| Type files | camelCase | `employee.ts` |
| API route files | Always named `route.ts` | `route.ts` |
| Context files | PascalCase with Context suffix | `AuthContext.tsx` |
| Service files | camelCase with Service suffix | `employeeService.ts` |

### Variables and Functions

| Item | Convention | Example |
|---|---|---|
| React components | PascalCase | `KPICard`, `FilterDropdown` |
| Functions | camelCase | `formatCurrency()`, `getAttritionRate()` |
| Constants | UPPER_SNAKE_CASE | `MAX_PAGE_SIZE`, `DEFAULT_THEME` |
| Types/Interfaces | PascalCase | `Employee`, `DashboardKPIs` |
| Enum values | PascalCase | `UserRole.CEO`, `UserRole.Manager` |
| Boolean variables | `is` or `has` prefix | `isLoading`, `hasError`, `isAttrited` |
| Event handlers | `handle` prefix | `handleFilterChange`, `handleExport` |

---

## 4. TypeScript Rules

- **Always use TypeScript.** No `.js` files except config files.
- **No `any` type.** If you don't know the type, use `unknown` and narrow it.
- **Define all API response types** in `/types/api.ts`.
- **Use interfaces for object shapes**, types for unions and primitives.
- **All component props must be typed** with an interface or inline type.

```typescript
// ✅ Good
interface KPICardProps {
  title: string
  value: number | string
  icon: LucideIcon
  isLoading?: boolean
  trend?: number
}

export function KPICard({ title, value, icon: Icon, isLoading, trend }: KPICardProps) {
  ...
}

// ❌ Bad
export function KPICard(props: any) {
  ...
}
```

---

## 5. React & Component Rules

### Component Structure Order
Every component file follows this order:
1. Imports
2. Type definitions (interfaces)
3. Constants (if any)
4. Component function
5. Export

```typescript
// 1. Imports
import { useState } from 'react'
import { Users } from 'lucide-react'

// 2. Types
interface KPICardProps {
  title: string
  value: string | number
}

// 3. Constants (if needed)
const DEFAULT_TREND = 0

// 4. Component
export function KPICard({ title, value }: KPICardProps) {
  const [isHovered, setIsHovered] = useState(false)

  return (
    <div>
      <p>{title}</p>
      <p>{value}</p>
    </div>
  )
}

// No default export for components — use named exports
```

### Component Rules
- Use **named exports** for all components (not `export default`)
- **One component per file**
- Keep components **under 150 lines** — split into smaller components if larger
- **No business logic in components** — use hooks and services
- **No direct Supabase calls in components** — use hooks
- **No `useEffect` for data fetching** — use SWR hooks

---

## 6. Data Fetching Rules

All data fetching uses **SWR hooks** defined in `/hooks/`.

```typescript
// hooks/useDashboardKPIs.ts

import useSWR from 'swr'
import type { DashboardKPIs } from '@/types/api'

const fetcher = (url: string) =>
  fetch(url).then(res => {
    if (!res.ok) throw new Error('Failed to fetch')
    return res.json()
  })

export function useDashboardKPIs(filters: Filters) {
  const params = new URLSearchParams()
  if (filters.department_id) params.set('department_id', filters.department_id)

  const { data, error, isLoading, mutate } = useSWR<{ data: DashboardKPIs }>(
    `/api/dashboard/kpis?${params.toString()}`,
    fetcher,
    { revalidateOnFocus: false }
  )

  return {
    kpis: data?.data ?? null,
    isLoading,
    isError: !!error,
    refresh: mutate,
  }
}
```

---

## 7. API Route Rules

Every API route handler follows this pattern:

```typescript
// app/api/employees/route.ts

import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import { NextRequest, NextResponse } from 'next/server'

export async function GET(req: NextRequest) {
  // 1. Auth check — always first
  const supabase = createRouteHandlerClient({ cookies })
  const { data: { session } } = await supabase.auth.getSession()

  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  // 2. Role check (if needed)
  const role = session.user.user_metadata?.role
  if (!['ceo', 'tester'].includes(role)) {
    return NextResponse.json({ error: 'Forbidden' }, { status: 403 })
  }

  // 3. Parse query params
  const { searchParams } = new URL(req.url)
  const page = parseInt(searchParams.get('page') ?? '1')
  const limit = parseInt(searchParams.get('limit') ?? '25')

  // 4. Query database
  try {
    const { data, count, error } = await supabase
      .from('employees')
      .select('*, departments(name), roles(name)', { count: 'exact' })
      .range((page - 1) * limit, page * limit - 1)

    if (error) throw error

    return NextResponse.json({ data, count, page, limit, status: 'success' })
  } catch (error) {
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 })
  }
}
```

---

## 8. Tailwind CSS Rules

- **Only use Tailwind utility classes.** No custom CSS unless absolutely necessary.
- **Dark mode** via `dark:` prefix: `className="bg-white dark:bg-gray-900"`
- **Responsive** via breakpoint prefix: `className="grid-cols-1 md:grid-cols-2 lg:grid-cols-4"`
- **No inline styles** (`style={{ }}`) unless for dynamic values that cannot be expressed in Tailwind
- **Class order:** layout → sizing → spacing → color → typography → border → effect → animation

```tsx
// ✅ Good
<div className="flex items-center gap-4 p-6 bg-white dark:bg-gray-800 rounded border border-gray-200 dark:border-gray-700 shadow-sm transition-all duration-200">

// ❌ Bad — random order, mixed concerns
<div style={{ padding: 24 }} className="bg-white flex border">
```

Use `clsx` or `cn` (from Shadcn) for conditional classes:
```tsx
import { cn } from '@/lib/utils'

<div className={cn(
  "p-4 rounded border",
  isActive && "border-blue-600 bg-blue-50",
  isError && "border-red-600 bg-red-50"
)}>
```

---

## 9. Error Handling

Every async operation must handle errors:

```typescript
// ✅ API routes: always use try/catch
try {
  const { data, error } = await supabase.from('employees').select('*')
  if (error) throw error
  return NextResponse.json({ data })
} catch (err) {
  console.error('[API] /employees error:', err)
  return NextResponse.json({ error: 'Failed to fetch employees' }, { status: 500 })
}

// ✅ Components: use isError state from SWR
if (isError) return <ErrorState message="Failed to load data" onRetry={refresh} />

// ✅ User actions: toast notifications
import { toast } from 'sonner'

const handleExport = async () => {
  try {
    await exportCSV(filters)
    toast.success('Export downloaded successfully')
  } catch {
    toast.error('Export failed. Please try again.')
  }
}
```

---

## 10. Git Commit Messages

Follow the Conventional Commits standard:

```
type(scope): short description

Types:
  feat     → New feature
  fix      → Bug fix
  style    → Styling/UI change (no logic change)
  refactor → Code restructure (no feature change)
  test     → Adding or fixing tests
  docs     → Documentation changes
  chore    → Config, dependencies, build changes
```

**Examples:**
```
feat(auth): implement login page with Supabase Auth
feat(ceo): add attrition by department chart
fix(middleware): correct role redirect for manager users
style(navbar): update mobile hamburger menu
refactor(kpi): extract KPICard into shared component
test(api): add unit tests for employees endpoint
docs: update README with setup instructions
chore: update Tailwind to v3.4.1
```

---

## 11. Code Quality Rules

- **ESLint** runs on every save (VS Code ESLint extension)
- **Prettier** formats on every save (VS Code Prettier extension)
- `npm run lint` must pass before any pull request
- `npm run type-check` must pass before any pull request
- **No `console.log`** in production code — use `console.error` only for actual errors in API routes
- **No commented-out code** in pull requests — delete it or use a GitHub issue
- **No TODO comments** in merged code — create a GitHub issue instead

---

## 12. VS Code Setup

Install these extensions for the best development experience:

| Extension | ID |
|---|---|
| ESLint | `dbaeumer.vscode-eslint` |
| Prettier | `esbenp.prettier-vscode` |
| Tailwind CSS IntelliSense | `bradlc.vscode-tailwindcss` |
| TypeScript Importer | `pmneo.tsimporter` |
| GitLens | `eamodio.gitlens` |

Add this to `.vscode/settings.json`:
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "tailwindCSS.experimental.classRegex": [
    ["cn\\(([^)]*)\\)", "'([^']*)'"]
  ]
}
```

---

## 13. What NOT to Do

| ❌ Don't | ✅ Do Instead |
|---|---|
| Call Supabase directly from a component | Use a custom SWR hook |
| Use `any` type | Define the proper type |
| Hardcode department names or IDs | Fetch from database or use constants |
| Expose `SUPABASE_SERVICE_ROLE_KEY` to the client | Server-side only, never in `NEXT_PUBLIC_*` |
| Use `export default` for components | Use named exports |
| Write CSS in `.css` files | Use Tailwind utility classes |
| Nest more than 3 levels of components inline | Extract into separate component files |
| Skip the loading state | Always show skeleton while data fetches |
| Skip the error state | Always handle `isError` from SWR |
| Store sensitive data in localStorage | Use Supabase session only |
