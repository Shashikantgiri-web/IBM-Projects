# 10 — State Management
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

EPIS uses a lightweight state management approach. There is no Redux or Zustand. State is managed through:
- **React Context** — for global state (auth, theme)
- **React useState / useReducer** — for local component state (filters, pagination)
- **SWR or React Query** — for server state (API data, caching)
- **localStorage** — for persistent user preferences (theme)

---

## 2. State Categories

| State | Type | Where Managed |
|---|---|---|
| Authentication session | Global | Supabase + React Context |
| Current user role | Global | Supabase session / Context |
| Theme (dark/light) | Global | React Context + localStorage |
| Active department filter | Local | Dashboard component |
| Active gender filter | Local | Dashboard component |
| Active job role filter | Local | Dashboard component |
| Employee search query | Local | Table component |
| Current page (pagination) | Local | Table component |
| Chart data | Server | SWR cache |
| Employee list data | Server | SWR cache |
| KPI data | Server | SWR cache |
| Loading state | Local | Component useState |
| Error state | Local | Component useState |
| Sidebar open/closed | Local | Layout component |
| Modal open/closed | Local | Component useState |
| Export in progress | Local | Component useState |

---

## 3. Auth Context

Wraps the entire application. Provides session, user, and role to any component.

```typescript
// context/AuthContext.tsx

interface AuthContextType {
  session: Session | null
  user: User | null
  role: 'ceo' | 'manager' | 'employee' | 'tester' | null
  loading: boolean
  signOut: () => Promise<void>
}

const AuthContext = createContext<AuthContextType>({...})

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [session, setSession] = useState<Session | null>(null)
  const [role, setRole] = useState<string | null>(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session)
      setRole(session?.user?.user_metadata?.role ?? null)
      setLoading(false)
    })

    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (_event, session) => {
        setSession(session)
        setRole(session?.user?.user_metadata?.role ?? null)
      }
    )

    return () => subscription.unsubscribe()
  }, [])

  const signOut = async () => {
    await supabase.auth.signOut()
    setSession(null)
    setRole(null)
  }

  return (
    <AuthContext.Provider value={{ session, user: session?.user ?? null, role, loading, signOut }}>
      {children}
    </AuthContext.Provider>
  )
}

export const useAuth = () => useContext(AuthContext)
```

---

## 4. Theme Context

Manages dark/light mode globally.

```typescript
// context/ThemeContext.tsx

interface ThemeContextType {
  theme: 'light' | 'dark'
  toggleTheme: () => void
}

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<'light' | 'dark'>(() => {
    if (typeof window !== 'undefined') {
      return (localStorage.getItem('epis-theme') as 'light' | 'dark') || 'light'
    }
    return 'light'
  })

  useEffect(() => {
    document.documentElement.classList.toggle('dark', theme === 'dark')
    localStorage.setItem('epis-theme', theme)
  }, [theme])

  const toggleTheme = () => setTheme(t => t === 'light' ? 'dark' : 'light')

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export const useTheme = () => useContext(ThemeContext)
```

---

## 5. Dashboard Filter State

Filters live in the dashboard page component. When a filter changes, SWR re-fetches with the new query parameters.

```typescript
// app/(dashboard)/ceo/page.tsx

interface Filters {
  department_id: string | null
  gender: string | null
  job_role_id: string | null
  education_id: string | null
}

const [filters, setFilters] = useState<Filters>({
  department_id: null,
  gender: null,
  job_role_id: null,
  education_id: null,
})

const updateFilter = (key: keyof Filters, value: string | null) => {
  setFilters(prev => ({ ...prev, [key]: value }))
}
```

---

## 6. Server State with SWR

SWR is used for all data fetching. It provides:
- Automatic revalidation on focus
- Cache deduplication
- Loading and error states
- Easy key-based re-fetching when filters change

```typescript
// hooks/useDashboardKPIs.ts

import useSWR from 'swr'

const fetcher = (url: string) => fetch(url).then(res => res.json())

export function useDashboardKPIs(filters: Filters) {
  const params = new URLSearchParams()
  if (filters.department_id) params.set('department_id', filters.department_id)
  if (filters.gender) params.set('gender', filters.gender)

  const { data, error, isLoading } = useSWR(
    `/api/dashboard/kpis?${params.toString()}`,
    fetcher,
    { revalidateOnFocus: false }
  )

  return {
    kpis: data?.data,
    isLoading,
    isError: !!error,
  }
}
```

---

## 7. Pagination State

```typescript
// components/DataTable.tsx

const [page, setPage] = useState(1)
const [limit] = useState(25)

const { data, isLoading } = useEmployees({ ...filters, page, limit })

const totalPages = Math.ceil((data?.count ?? 0) / limit)
```

---

## 8. Loading & Error States

Every data-dependent component handles its own loading and error states.

**Loading:** Show skeleton UI while `isLoading` is true  
**Error:** Show toast notification and retry button  
**Empty:** Show empty state illustration when data returns but count is 0

```typescript
// Standard pattern for every data component

if (isLoading) return <SkeletonCard />
if (isError) return <ErrorState message="Failed to load data" onRetry={mutate} />
if (!data || data.length === 0) return <EmptyState message="No employees found" />

return <DataTable data={data} />
```

---

## 9. Sidebar Collapse State

```typescript
// components/layout/DashboardLayout.tsx

const [sidebarOpen, setSidebarOpen] = useState(true)

// Mobile: sidebar starts closed
useEffect(() => {
  if (window.innerWidth < 768) setSidebarOpen(false)
}, [])
```

---

## 10. Root Provider Setup

All providers are wrapped in order in the root layout:

```typescript
// app/layout.tsx

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body>
        <AuthProvider>
          <ThemeProvider>
            <SWRConfig value={{ revalidateOnFocus: false }}>
              {children}
              <Toaster />
            </SWRConfig>
          </ThemeProvider>
        </AuthProvider>
      </body>
    </html>
  )
}
```
